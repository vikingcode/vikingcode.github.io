---
layout: page
title: Windows 8 notifications from WPF
---

Windows 8 introduces some nice new Windows APIs - some are 'WinRT' only, some are not. One that is accessible to both is the Notification (Toast) API.

It required a bit of trial and error, but Shiftkey's [BoxKite.Notifications](https://github.com/shiftkey/BoxKite.Notifications) has a pending [pull request](https://github.com/shiftkey/BoxKite.Notifications/pull/1) for .NET 4.5 support. 

##Caveats
There are some caveats. With no AppXManifest to define settings like what WinRT apps do, you cannot

* set the background colour of a notification
* set the 'image' on `TextAndImage` templates to a remote location (without the AppXManifest giving you permission to go online, the notification won't go online)
* your application *must* have a shortcut in the Start Menu

##Using BoxKite.Notifications
1. You'll need to build BoxKite.Notifications from source. 
2. You need to modify your csproj by hand (I couldn't see a way to do it from the GUI) to included `<TargetPlatformVersion>8.0</TargetPlatformVersion>`
3. That will then give you a new option in Reference Manager, where you can add a reference to... Windows.  
![](images/postimages/reference-manager-windows.png)

4. You will also need to add references to ` <Reference Include="System.Runtime" /> <Reference Include="System.Runtime.InteropServices.WindowsRuntime" />` (again, I did this by hand)

5. Use the ShortcutHelper (below, code originally from [MSDN](http://code.msdn.microsoft.com/windowsdesktop/Sending-toast-notifications-71e230a2)) to register your app.     
`string AppId = "myawesomeapp";`  
`ShortcutHelper.TryCreateShortcut(AppId);`

6. Create and send your toast.  
{% highlight csharp %}
var toast = BoxKite.Notifications.Templates.ToastContentFactory.CreateToastImageAndText02();
toast.TextHeading.Text = status.User.ScreenName;
toast.TextBodyWrap.Text = status.Text;
toast.Image.Alt = "Web image";
toast.Image.Src = "file://" + image;
var actualToast = toast.CreateNotification();
actualToast.Activated += (s, e) =>
{
	MessageBox.Show("User clicked toast");
};
ToastNotificationManager.CreateToastNotifier(AppId).Show(actualToast);
{% endhighlight %}


##ShortcutHelper
{% highlight csharp %}

public static class ShortcutHelper
{
    private static string AppId;
    public static bool TryCreateShortcut(string appid)
    {
        AppId = appid;
        String shortcutPath = Environment.GetFolderPath(Environment.SpecialFolder.ApplicationData) + "\\Microsoft\\Windows\\Start Menu\\Programs\\Beagle.lnk";
        if (!File.Exists(shortcutPath))
        {
            InstallShortcut(shortcutPath);
            return true;
        }
        return false;
    }

    private static void InstallShortcut(String shortcutPath)
    {
        // Find the path to the current executable
        String exePath = Process.GetCurrentProcess().MainModule.FileName;
        var newShortcut = (IShellLinkW)new CShellLink();

        // Create a shortcut to the exe
        ErrorHelper.VerifySucceeded(newShortcut.SetPath(exePath));
        ErrorHelper.VerifySucceeded(newShortcut.SetArguments(""));

        // Open the shortcut property store, set the AppUserModelId property
        var newShortcutProperties = (IPropertyStore)newShortcut;

        using (var appId = new PropVariant(AppId))
        {
            ErrorHelper.VerifySucceeded(newShortcutProperties.SetValue(SystemProperties.System.AppUserModel.ID, appId));
            ErrorHelper.VerifySucceeded(newShortcutProperties.Commit());
        }

        // Commit the shortcut to disk
        var newShortcutSave = (IPersistFile)newShortcut;

        ErrorHelper.VerifySucceeded(newShortcutSave.Save(shortcutPath, true));
    }

    internal enum STGM : long
    {
        STGM_READ = 0x00000000L,
        STGM_WRITE = 0x00000001L,
        STGM_READWRITE = 0x00000002L,
        STGM_SHARE_DENY_NONE = 0x00000040L,
        STGM_SHARE_DENY_READ = 0x00000030L,
        STGM_SHARE_DENY_WRITE = 0x00000020L,
        STGM_SHARE_EXCLUSIVE = 0x00000010L,
        STGM_PRIORITY = 0x00040000L,
        STGM_CREATE = 0x00001000L,
        STGM_CONVERT = 0x00020000L,
        STGM_FAILIFTHERE = 0x00000000L,
        STGM_DIRECT = 0x00000000L,
        STGM_TRANSACTED = 0x00010000L,
        STGM_NOSCRATCH = 0x00100000L,
        STGM_NOSNAPSHOT = 0x00200000L,
        STGM_SIMPLE = 0x08000000L,
        STGM_DIRECT_SWMR = 0x00400000L,
        STGM_DELETEONRELEASE = 0x04000000L,
    }

    internal static class ShellIIDGuid
    {
        internal const string IShellLinkW = "000214F9-0000-0000-C000-000000000046";
        internal const string CShellLink = "00021401-0000-0000-C000-000000000046";
        internal const string IPersistFile = "0000010b-0000-0000-C000-000000000046";
        internal const string IPropertyStore = "886D8EEB-8CF2-4446-8D02-CDBA1DBDCF99";
    }

    [ComImport, Guid(ShellIIDGuid.IShellLinkW), InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
    internal interface IShellLinkW
    {
        UInt32 GetPath([Out(), MarshalAs(UnmanagedType.LPWStr)] StringBuilder pszFile, int cchMaxPath, IntPtr pfd, uint fFlags);
        UInt32 GetIDList(out IntPtr ppidl);
        UInt32 SetIDList(IntPtr pidl);
        UInt32 GetDescription([Out(), MarshalAs(UnmanagedType.LPWStr)] StringBuilder pszFile, int cchMaxName);
        UInt32 SetDescription([MarshalAs(UnmanagedType.LPWStr)] string pszName);
        UInt32 GetWorkingDirectory([Out(), MarshalAs(UnmanagedType.LPWStr)] StringBuilder pszDir, int cchMaxPath);
        UInt32 SetWorkingDirectory([MarshalAs(UnmanagedType.LPWStr)] string pszDir);
        UInt32 GetArguments([Out(), MarshalAs(UnmanagedType.LPWStr)] StringBuilder pszArgs, int cchMaxPath);
        UInt32 SetArguments([MarshalAs(UnmanagedType.LPWStr)] string pszArgs);
        UInt32 GetHotKey(out short wHotKey);
        UInt32 SetHotKey(short wHotKey);
        UInt32 GetShowCmd(out uint iShowCmd);
        UInt32 SetShowCmd(uint iShowCmd);
        UInt32 GetIconLocation([Out(), MarshalAs(UnmanagedType.LPWStr)] out StringBuilder pszIconPath, int cchIconPath, out int iIcon);
        UInt32 SetIconLocation([MarshalAs(UnmanagedType.LPWStr)] string pszIconPath, int iIcon);
        UInt32 SetRelativePath([MarshalAs(UnmanagedType.LPWStr)] string pszPathRel, uint dwReserved);
        UInt32 Resolve(IntPtr hwnd, uint fFlags);
        UInt32 SetPath([MarshalAs(UnmanagedType.LPWStr)] string pszFile);
    }

    [ComImport, Guid(ShellIIDGuid.IPersistFile), InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
    internal interface IPersistFile
    {
        UInt32 GetCurFile([Out(), MarshalAs(UnmanagedType.LPWStr)] StringBuilder pszFile);
        UInt32 IsDirty();
        UInt32 Load([MarshalAs(UnmanagedType.LPWStr)] string pszFileName, [MarshalAs(UnmanagedType.U4)] STGM dwMode);
        UInt32 Save([MarshalAs(UnmanagedType.LPWStr)] string pszFileName, bool fRemember);
        UInt32 SaveCompleted([MarshalAs(UnmanagedType.LPWStr)] string pszFileName);
    }

    [ComImport]
    [Guid(ShellIIDGuid.IPropertyStore)]
    [InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
    interface IPropertyStore
    {
        UInt32 GetCount([Out] out uint propertyCount);
        UInt32 GetAt([In] uint propertyIndex, out PropertyKey key);
        UInt32 GetValue([In] ref PropertyKey key, [Out] PropVariant pv);
        UInt32 SetValue([In] ref PropertyKey key, [In] PropVariant pv);
        UInt32 Commit();
    }

    [ComImport, Guid(ShellIIDGuid.CShellLink), ClassInterface(ClassInterfaceType.None)]
    internal class CShellLink { }

    public static class ErrorHelper
    {
        public static void VerifySucceeded(UInt32 hresult)
        {
            if (hresult > 1)
            {
                throw new Exception("Failed with HRESULT: " + hresult.ToString("X"));
            }
        }
    }
}

{% endhighlight %}
