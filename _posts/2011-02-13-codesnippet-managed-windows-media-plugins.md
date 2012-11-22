--- 
layout: page
title: "Codesnippet: Managed Windows Media Player plugins"
description: This is first of a three part series. The following posts will discuss installing the plugin and using WCF for IPC
funnelweb_id: 72
date: 2011-02-13 13:00:00 +11:00
tags: "c# programming codesnippet code .net "
comments: true
---
![Now Playing][1]

As I'm knocking off various features in [MahChats][2], it occurred to me that eventually I'd need to interact with a media player of some sort. Given that I use Windows Media Player (WMP) and it will be install on the machine of all MahChats users, it sounded like a good start.

Live Messenger and GTalk both ship with WMP "Background" plugins that send data about the currently playing track to the respective programs. There are actually several types of plugin interfaces in WMP:

 - **Background**; non-interactive tasks, in most cases this is sending the "Now Playing" information somewhere, such as Last.fm, [to Xml][3], or to IM clients.
Typically these plugins have no UI, except (possibly) a settings window.
 - **DSP** (Digital Sound Processing); alter the sound being played back.
 - **Visualisation**; they're the funky effects that react to what music is playing, such as the [Winter Visualisation on Coding4Fun][4]

[Ianier Munoz][5] many years ago created a few articles that made their way up onto MSDN-Luxemberg, but since then Microsoft has taken the entire blog down. Thankfully, Google has the content cached, and I was able to make my starting point by mixing up [Extend Windows Media Player with Visual C++ Express and Advanced COM Interop][6] and [Writing A Complete DSP Plug-in for Windows Media Player 9 in C#][7]

## COM Interop ##
The basic idea is to create a C# layer that reimplements the COM interfaces. As this codesnippet is about background plugins, not too much has to be recreated.

**IWMPPluginUI**  
It seems that all plugin types will use this as the initial creation. For a background plugin, most of the code will go in an implementation of this interface.

    [ComVisible(true)]
    [InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
    [Guid("4C5E8F9F-AD3E-4bf9-9753-FCD30D6D38DD")]
    public interface IWMPPluginUI
    {
        void SetCore(WMPCore pCore);
        IntPtr Create(IntPtr hwndParent);
        void Destroy();
        void DisplayPropertyPage(IntPtr hwndParent);
        object GetProperty(string pwszName);
        void SetProperty(string pwszName, [In] ref object pvarProperty);
        void TranslateAccelerator(IntPtr lpmsg);
    }

**Registration**  
This interface is used when registering the plugin with WMP.


    [ComImport]
    [InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
    [Guid("68E27045-05BD-40b2-9720-23088C78E390")]
    public interface IWMPMediaPluginRegistrar
    {
        void WMPRegisterPlayerPlugin(string pwszFriendlyName, string pwszDescription, string pwszUninstallString, uint dwPriority, Guid guidPluginType, Guid clsid, uint cMediaTypes, [MarshalAs(UnmanagedType.LPArray)] Guid[] pMediaTypes);
        void WMPUnRegisterPlayerPlugin(Guid guidPluginType, Guid clsid);
    }

**Enums**

    [ComVisible(true)]
    public enum PluginType : int
    {
        PLUGIN_TYPE_BACKGROUND = 1,
        PLUGIN_TYPE_SEPARATEWINDOW = 2,
        PLUGIN_TYPE_DISPLAYAREA = 3,
        PLUGIN_TYPE_SETTINGSAREA = 4,
        PLUGIN_TYPE_METADATAAREA = 5
    }

## Plugin implementation ##
Because I'm wanting to know what state WMP is in, I needed an enum to tell me if it was playing, paused or stopped. Only if it is playing do I want to send the information about the song - if it's paused or stopped I want to clear it!

    public enum PlayStateEnum
    {
        Stopped = 1,
        Paused = 2,
        Playing = 3,
        Changing = 9
    }

`WMPLib` is a COM reference. When adding COM references in Visual Studio, there are several matches for "Windows Media Player" and `WMPLib` should be the *first* result that is just "Windows Media Player".


    using WMPLib;

    [ComVisible(true)]
    [Guid("<YOUR GUID GOES HERE>")]
    public class WmpNowPlayingPlugin : IWMPPluginUI
    {
        public static string RegPath { get { return Path.Combine(Registration.PLUGIN_INSTALLREGKEY, ClsidStr); } }
        public static string ComPath { get { return Path.Combine(@"CLSID\" + ClsidStr, "InprocServer32"); } }
        private static string ClsidStr { get { return typeof(WmpNowPlayingPlugin).GUID.ToString("B"); } }
        private WMPCore _mCore;

        [DllImport("user32.dll")]
        private static extern IntPtr SetParent(IntPtr hWndChild, IntPtr hWndNewParent);

        public WmpNowPlayingPlugin()
        {
 
        }

        public void SetCore(WMPCore pCore)
        {
            if (pCore != null)
            {
                _mCore = pCore;
                _mCore.PlayStateChange += _mCore_PlayStateChange;
            }
        }

        void _mCore_PlayStateChange(int NewState)
        {
            var state = ((PlayStateEnum)NewState);
            switch (state)
            {
                case PlayStateEnum.Playing:
                    var track = ((IWMPMedia3)_mCore.currentMedia);
                    string nowPlaying = String.Format("{0} - {1}", track.getItemInfo("Author"), track.name);
                    MessageBox.Show(nowPlaying);
                    break;
                case PlayStateEnum.Stopped:
                case PlayStateEnum.Paused:
                    //Clear status?
                    break;
            }
        }

        public IntPtr Create(IntPtr hwndParent)
        {
            return IntPtr.Zero;
        }

        public void Destroy()
        {

        }
        public void DisplayPropertyPage(IntPtr hwndParent)
        {
            MessageBox.Show("This plug-in has no property page");
        }

        public object GetProperty(string pwszName)
        {
            return null;
        }

        public void SetProperty(string pwszName, [In] ref object pvarProperty)
        {
        }

        public void TranslateAccelerator(IntPtr lpmsg)
        {
        }
    }

In `_mCore_PlayStateChange` you'll notice that I was able to get the track name from the name property, but I had to query (using `getItemInfo`) to get the "Author" (artist in this case). If you're just after those two values, it's pretty straight forward, but you may want further details. If so, [there is a list on MSDN][8] with all the values that you can use `getItemInfo` to obtain.

You could also listen in to the `CurrentItemChange` event, but that will trigger on less information, such as when nothing is being played but the user has hit next or previous.


## Exposing plugin to WMP ##
Now that you've got a plugin created, how do you actually go about making WMP recognise it? If you'd been paying attention, you would have noticed a call to `Registration.PLUGIN_INSTALLREGKEY` in `WmpNowPlayingPlugin`. This helper class holds a bunch of Registry keys for both the installer and plugin itself.

    public class Registration
    {
        // UI plug-ins
        public const string PLUGIN_INSTALLREGKEY = @"Software\Microsoft\MediaPlayer\UIPlugins";
        public const string PLUGIN_INSTALLREGKEY_FRIENDLYNAME = "FriendlyName";
        public const string PLUGIN_INSTALLREGKEY_DESCRIPTION = "Description";
        public const string PLUGIN_INSTALLREGKEY_CAPABILITIES = "Capabilities";
        public const string PLUGIN_INSTALLREGKEY_UNINSTALL = "UninstallPath";

        public static readonly Guid CLSID_WMPMediaPluginRegistrar = new Guid("5569e7f5-424b-4b93-89ca-79d17924689a");

        public static IWMPMediaPluginRegistrar CreateWMPMediaPluginRegistrar()
        {
            return (IWMPMediaPluginRegistrar)Activator.CreateInstance(Type.GetTypeFromCLSID(CLSID_WMPMediaPluginRegistrar));
        }
    }

The actual "install" process is through `System.Configuration.Install`

    using System;
    using System.Collections;
    using System.ComponentModel;
    using System.Configuration.Install;
    using System.Runtime.InteropServices;
    using System.Text;
    using System.Threading;
    using Microsoft.Win32;
    
    [RunInstaller(true)]
    public class WmpNowPlayingPluginInstaller : Installer
    {
        private readonly Container components;

        public WmpNowPlayingPluginInstaller()
        {
            InitializeComponent();
        }

        [DllImport("kernel32.dll")]
        private static extern int SearchPath(string lpPath, string lpFileName, string lpExtension, int nBufferLength, [Out] StringBuilder lpBuffer, out IntPtr lpFilePart);

        protected override void Dispose(bool disposing)
        {
            if (disposing)
            {
                if (components != null)
                {
                    components.Dispose();
                }
            }
            base.Dispose(disposing);
        }

        private void InitializeComponent()
        {
        }

        private static string SearchPath(string fileName)
        {
            int len = 256;
            IntPtr dummy;
            var b = new StringBuilder(len);
            len = SearchPath(null, fileName, null, len, b, out dummy);
            b.Length = len;
            return b.ToString();
        }

        private void RunSta(ThreadStart method)
        {
            var t = new Thread(method);
            t.ApartmentState = ApartmentState.STA;
            t.Start();
            t.Join();
        }

        public override void Install(IDictionary stateSaver)
        {
            base.Install(stateSaver);

            using (RegistryKey key = Registry.ClassesRoot.OpenSubKey(WmpNowPlayingPlugin.ComPath, true))
            {
                var lib = (string) key.GetValue(null);
                key.SetValue(null, SearchPath(lib));
            }


            // Install UI plug-in
            using (RegistryKey key = Registry.LocalMachine.CreateSubKey(WmpNowPlayingPlugin.RegPath))
            {
                key.SetValue(Registration.PLUGIN_INSTALLREGKEY_CAPABILITIES, (int) PluginType.PLUGIN_TYPE_BACKGROUND);
                key.SetValue(Registration.PLUGIN_INSTALLREGKEY_FRIENDLYNAME, "MahChats Now Playing");
                key.SetValue(Registration.PLUGIN_INSTALLREGKEY_DESCRIPTION, "Now Playing Plugin for MahChats");
            }
        }

        public override void Uninstall(IDictionary savedState)
        {
            base.Uninstall(savedState);

            // Uninstall UI plug-in
            Registry.LocalMachine.DeleteSubKey(WmpNowPlayingPlugin.RegPath, false);
        }
    }

Once compiled, at this stage you can install/uninstall the plugin using the command line `InstallUtil wmpnowplaying.dll` (and `InstallUtil /u wmpnowplaying.dll` to uninstall).

## Show Me The Code! ##
No! I'm not going to create a separate code package for it at this stage, but you can [download the source inside the MahChats Mercurial repo][9]

## Next up ##
While this is little more than a 'hello world' for WMP Plugins, the next post will show how I've used it in a real situation to get data back to my app using WCF's Named Pipes for inter-process communication (IPC).


  [1]: /images/postimages/nowplaying.png
  [2]: http://www.mahchats.com
  [3]: http://brandon.fuller.name/archives/hacks/nowplaying/wmp/
  [4]: http://channel9.msdn.com/coding4fun/articles/Winter-Visualization-for-Windows-Media-Player-in-C
  [5]: http://www.chronotron.com/
  [6]: http://webcache.googleusercontent.com/search?q=cache:q81xD9WqoY4J:www.microsoft.com/belux/msdn/fr/community/columns/munoz/extendwmp_part1.mspx+http://www.microsoft.com/belux/msdn/nl/community/columns/munoz/extendwmp_part1.mspx&cd=1&hl=en&ct=clnk&gl=au&source=www.google.com.au
  [7]: http://webcache.googleusercontent.com/search?q=cache:c-E_gTnOk-wJ:www.microsoft.com/belux/msdn/fr/community/columns/munoz/wmpphaser.mspx+http://www.microsoft.com/belux/msdn/nl/community/columns/munoz/wmpphaser.mspx&cd=1&hl=en&ct=clnk&gl=au&source=www.google.com.au9595125
  [8]: http://msdn.microsoft.com/en-us/library/dd562379(v=VS.85).aspx
  [9]: http://code.jenkinsfamily.com.au/mahchats/src/681190e49344/WMPNowPlaying/
