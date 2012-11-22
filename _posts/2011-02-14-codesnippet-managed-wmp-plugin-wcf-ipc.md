--- 
layout:  page
title: "Codesnippet: Managed WMP plugin: WCF/IPC"
description: "This is second of a three part series, this part discusses using WCF for IPC with our WMP Now Playing plugin"
funnelweb_id: 74
date: 2011-02-14 13:00:00 +11:00
tags: "c# programming .net codesnippet wcf "
comments: true
---
It's all well and good that we can now [detect what you're listening to in Windows Media Player][1], but what can we actually do with it? If you were running a web service that only used WMP, you could have all your sending of data in there - but a better approach is to communicate with another application which controls what to do with the data (Last.fm use this approach for their Scrobbler.)

So how do you go about communicating with other apps on the same system? You could write to registry or to a file and monitor it for changes, setup a web server locally, or more sensibly use a proper *inter-process communication* such as *named pipes*.  
Simply put, named pipes are a form of communication between different apps on the same system - that is, it exists in memory and isn't accessible over LAN/WAN. The way to avoid collisions between different sets apps from interrupting each other is to give the pipe a unique name.

For .NET, the easiest way to do IPC/Named pipes is with WCF. With WCF, you need/should define your "contract"/interface so it can be easily accessible from the listener (in this case, MahChats) and sender (WMPNowPlaying)

    [ServiceContract(Namespace = "http://mahchats.com")]
    public interface IMusicProvider
    {
        [OperationContract]
        void Playing(string Artist, string Track);

        [OperationContract]
        void Stopped();
    }

While it is a very basic interface, not too much data needs to be transfered around. If you've not used WCF before, the ServiceContract "describe which operations you can perform on the service" (see [CODE Magazine's WCF Primer][2])

## Consumption (inside WmpNowPlayingPlugin.cs) ##

First you'll need to setup a binding and create a proxy class to access the WCF Named Pipe. In the `SetCore` method (which tells the plugin it is enabled) of `WmpNowPlayingPlugin`:


    EndpointAddress ep = new EndpointAddress("net.pipe://localhost/mahchats/music");

    NetNamedPipeBinding binding = new NetNamedPipeBinding(NetNamedPipeSecurityMode.None);
    proxy = ChannelFactory<IMusicProvider>.CreateChannel(binding, ep);


Once you've setup your connection, all you need to do is call `Playing(.., ..)` (and/or `Stopped()`). The best spot for this is `_mCore_PlayStateChanged` replacing the `MessageBox.Show`:

    void _mCore_PlayStateChange(int NewState) {
    ...
            proxy.Playing(track.getItemInfo("Author"), track.name);
    ...
    }

## Consumption (inside MahChats) ##
To setup a WCF host, there are two portions - the contract implementation, and the bindings that start the service. 

First up, the implementation

    public class MusicService : IMusicProvider
    {
        public void Playing(string artist, string track)
        {
            var connectionManager = CompositionManager.Get<IConnectionManager>();

            var song = new Track()
            {
                Artist = artist,
                Title = track
            };

            foreach (var n in connectionManager.Networks)
                n.SetRichPersonalMessage(song);
        }

        public void Stopped()
        {
            var connectionManager = CompositionManager.Get<IConnectionManager>();

            foreach (var n in connectionManager.Networks)
                n.SetRichPersonalMessage(null);
        }
    }

The implementation is as basic as you'd imagine - when a song comes in, iterate over all the configured/connected networks and set the status to the song. When it's stopped, clear the status message.

The binding/hosting portion is equally as easy to setup.

    var baseAddress = new Uri("net.pipe://localhost/mahchats/service");
    var address = "net.pipe://localhost/mahchats/music";

    NetNamedPipeBinding binding = new NetNamedPipeBinding(NetNamedPipeSecurityMode.None);

    var serviceHost = new ServiceHost(typeof (MusicService), baseAddress);
    serviceHost.AddServiceEndpoint(typeof(IMusicProvider), binding, address);
    serviceHost.Open();

Walking through that - the address (which we used in the WMPNowPlaying plugin) defines the named pipe endpoint. So long as it starts with `net.pipe://` it can be whatever you like, as long as the server and client match. 

The binding (of which there could be many) defines that it will be just a NamedPipe. You could also have a `BasicHttpBinding` so that it would be accessible via a browser, but in this case is completely useless.

The `ServiceHost` is what turns our `ServiceContract` into a WCF service, meaning no more writing of networking code.

Bonus points
------------

One of the issues with WCF's client proxy is that once the connection is "faulted", you have to dispose of it and spawn a new proxy. In the case of WMP/WMPNowPlaying running but MahChats not, the client will fault straight away. This just means a bunch of ugly exception handling, however, there is one good looking solution I've not had time to look into - [WCF Dynamic Client Proxy][3] which sells itself to me in a single line 

> **Does not have to be disposed if the channel fails. You can simply reuse it immediately.**


Show me the code
--------------------

Just like part one, the code for this series of blog posts [is available in the MahChats Mercurial repo][4]


## Next up ##
Using `InstallUtil` might work great for devs, but for end users it isn't an option. Next up I take a look at using Visual Studio's *Setup Project* for installing the plugin.

  [1]: codesnippet-managed-windows-media-plugins
  [2]: http://www.code-magazine.com/article.aspx?quickid=0605051&page=2
  [3]: http://wcfdynamicclient.codeplex.com/
  [4]: http://code.jenkinsfamily.com.au/mahchatsimmediately.**
