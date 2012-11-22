--- 
layout: page
title: "Building my HTPC: Software"
description: "Building HTPC: The Software"
funnelweb_id: 27
date: 2008-12-10 13:00:00 +11:00
tags: "buildinghtpc guide media-center "
comments: true
---
<p>Setting up a HTPC for the first time can be a bit of a daunting experience. There are so many hardware choices which seem so critical, and then you have to install and configure everything - it's times like this DVR/PVR's look appealing by only needing to be plugged into the TV and turned on.</p>
<p>This started out as a quick seven-ish step guide to Vista Media Center, but it just kept growing. As I discover (or other people point out) any steps or general information that is missing I'll continually update the article</p>
<ol>
<li>
<h4>Install Vista Home Premium or Ultimate.</h4>
<p>Both the 32bit or 64bit versions of Windows will work fine. For the record, I'm still using a 32bit for my HTPC, but on my desktop I'm running 64bit which plays back every file format I've tried.
</p></li><li>
<h4>(Optional) Vista Home Premium Network Share Interaction</h4>

<p>If you're installing Home Premium and using password protected network shares, you'll probably notice that upon every reboot (thanks to <strong>Chris Maye</strong>r for pointing this bug out) network shares will be lost, or you'll have to reenter network credentials to open a networked folder. Unfortunately, it isn't a "bug" as such, but a "by design feature" for Vista Home Basic and Premium.</p>
<p>The first fix everybody will point out is to go to the Windows Credentials Management tool in Control Panel -&gt; User Accounts, but unfortunately this is unavailable.</p>
<p>There are a few "solutions":</p>
<ul>
<li>Allow anonymous access to your media shares</li>
<li>Use a script on login, <em>net use \\server drive: /user:username password. </em>This means your password will now be stored in a plain text file.</li>

<li>Make your HTPC's user account match that of your network share. ie <em>\\yourserver</em> requires "foo/bar", so your HTPC's username is <em>foo</em>, with a password of <em>bar</em>. It will never prompt for the username or password.</li>
</ul>
<p>The last is the only real "secure" option, and also probably the easiest to setup. Combine that with auto-login (below), and the username/password aren't an issue.
</p></li><li>
<h4>(Optional) Enable Auto-User-Login</h4>
<p>If you've got a password on your HTPC's user account, by default it will prompt every time you turn the machine on, which is disastrous for machines that are operated by remote only. You can get around this by enabling auto-login.</p>
<p>Into the start menu type (minus the quotes), "control userpasswords2? to bring up the Advanced User Control Panel.</p>

<div style="text-align: center;"><img src="http://images.theleagueofpaul.com/postimages//controluserpasswords2.jpg" alt="" height="381" width="354"></div>
<p>Select the account you want to automatically logon, then untick "<em>Users must enter a user name and password to use this computer</em>", and a prompt will appear asking for the username and password.</p>
<p>(Thanks to <a href="http://www.kosertech.com/blog/">Vince Koser</a> for pointing out how useful this could be for a HTPC!)
</p></li><li>
<h4>Install all the latest drivers.</h4>
<p>Generally you can just discard the CD that came with your devices, as they're usually horribly out of date.<br>
If you don't have access to another computer with a USB key and Windows hasn't picked up your network card, you may need to use the motherboard/network card CD to install those drivers to get online.</p>
<ul>

<li>For motherboards, TV tuners, audio cards, etc, visit the manufacturers website.</li>
<li>For video cards (including integrated), the three players in the game are <a href="http://ati.amd.com/support/driver.HTML">ATI</a>, <a href="http://www.nvidia.com/">NVIDIA</a> and <a href="http://www.intel.com/">Intel</a>.</li>
</ul>
</li>
<li>
<h4>Setting up sound
<p><a rel="lightbox" href="http://images.theleagueofpaul.com/postimages//sound.jpg"><img style="border-width: 0px; display: block; float: none; margin-left: auto; margin-right: auto;" title="sound" src="http://images.theleagueofpaul.com/postimages//sound-thumb.jpg" alt="sound" border="0" height="332" width="299"></a></p></h4>
<p>Depending on both the capabilities of your HTPC and your sound systems, you need to set your default sound playback device appropriately.<br>

This dialogue can be found under Control Panel -&gt; Sound.<br>
Analogue outputs (usually a green 3.5mm stereo jack on your motherboard) will appear as speakers or headphones<br>
HDMI, Optical and Coax will all appear with a similar SPDIF/Digital Output Device.</p>
<p>Word of caution, updating your video drivers <strong>may</strong> reset your audio choice if the video card includes audio out over HDMI. This happened recently to me.
</p></li><li>
<h4>(Optional) Enable Remote Desktop.</h4>
<p>If you have Vista Ultimate, you can setup Remote Desktop so that you can perform maintenance without having to plug a keyboard and mouse into your HTPC.<img style="border-width: 0px; display: block; float: none; margin-left: auto; margin-right: auto;" title="remotedesktop" src="http://images.theleagueofpaul.com/postimages//remotedesktop.jpg" alt="remotedesktop" border="0" height="367" width="424"></p>

<p>Control Panel -&gt; System -&gt; Remote Settings -&gt; Allow connections
</p></li><li>
<h4>(Optional) Remove Password Policy For RDP</h4>
<p><em>Control Panel -&gt; Administrative Tools -&gt; Local Security Policy -&gt; Security Options</em></p>

<ol>If you chose to enable RDP, but don't want to have a password on your user account (which RDP requires by default), change "<em>Accounts: Limit local account use of blank passwords to console only</em>" to <em>Disabled</em>. Be warned, this obviously is a bit of a security risk, make sure you have other security procedures in place.<strong> It is easier/more secure just to enable auto-logon for the HTPC</strong>.</ol>
</li>
<li>
<h4>(Optional) Install/Configure VNC.</h4>
<p>VNC is an open source protocol for cross platform remote connections. Vista Home Premium doesn't come with Remote Desktop (it has the client but <em>not</em> the server portion), so this will do the same job as in step 3.

</p></li><li>
<h4>Install all Windows updates</h4>
<p>The big one to look out for is obviously Service Pack 1 for the fixes to file transfers, but generally get any and everything that's available.
</p></li><li>
<h4>Run/Configure Windows Media Player</h4>
<p>Windows Media Center and Media Player are very closely linked - you can't have the two of them playing separate files at the same time because WMC relies on WMP for playback.<a rel="lightbox" href="http://images.theleagueofpaul.com/postimages//wmp-rip.jpg"><img style="border-width: 0px; display: block; float: none; margin-left: auto; margin-right: auto;" title="wmp_rip" src="http://images.theleagueofpaul.com/postimages//wmp-rip-thumb.jpg" alt="wmp_rip" border="0" height="451" width="350"></a><br>
Ripping settings are probably the only that cannot be accessed within Media Center itself. In the Rip Music tab of Options, the highlighted values are what you need to change.</p>
<h4>Rip Options</h4>
<ul>
<li>
<h4>Rip Location</h4>

<p>Since I store my music on my server, I've changed the rip location to there, if you keep it on your HTPC, you might want to change partitions/etc.
</p></li><li>
<h4>Audio format</h4>
<p>Your choices are WMA, WMA Pro, WMA (Variable Bit Rate), WMA Lossless, MP3 or WAV.Personally, I use WMA VBR, because the quality is good enough but not as space consuming as WMA Lossless. I'm not suggesting either WMA or MP3 is a superior format, but not all MP3 encoders are created equally. If I was to use a MP3 encoder, it would be LAME, not a Microsoft one.
</p></li><li>
<h4>Audio Quality</h4>
<p>Given how cheap hard drives are these days, always push the slider towards the best quality end.
</p></li></ul>
</li>
<li>
<h4>Install codecs</h4>
<p>Codecs allow you to decode (and sometimes encode, the word codec is short for "compressor/decompressor", although often the encoding part isn't implemented) video and audio. Windows Vista ships with a very limited number of codecs (mp3, mpeg2, VC1/WMV9), so other common files like DivX/XVid, H.264 etc won't play back.Windows 7 will be shipping (and even the pre-release versions have them) with video for MPG4 (DivX/XviD) and H.264 as well as more advanced audio codecs - all my AVI files played back fine. However, the Matroska file format (MKV extension) is not natively supported, so something still needs to be installed for that to be played back.</p>
<p>My codec recommendations for Vista are:</p>

<ul>
<li><a href="http://ffdshow-tryout.sourceforge.net/">ffdshow</a> - either grab the latest tryouts, or the <a href="http://www.cccp-project.net/">CCCP</a> codec pack is a nice bundle/preconfigured set</li>
<li>and <a href="http://www.coreavc.com/">CoreAVC Pro</a>; its a very fast software H.264 decoder, although its not free (make sure you uncheck "Haali Media Splitter Install" disabled if CCCP is installed)</li>
</ul>
</li>
<li>
<h4>(Optional) Install HD-DVD/Bluray playback software.</h4>

<p>The options are <a href="http://www.cyberlink.com/prog/product/index.do?locale=en_US">PowerDVD Ultra</a>, <a href="http://www.corel.com/servlet/Satellite/au/en/Product/1189528458632#versionTabview=tab0&amp;tabview=tab0">WinDVDPlus</a>, or <a href="http://www.arcsoft.com/products/totalmediatheatre/">Total Media Theatre</a>. Only TMT has integration with Media Center where it places a launcher app, and when it closes restores Media Center. The latest versions of PowerDVD Ultra (v8+) <strong>do not</strong> playback HD-DVD's anymore.</p>
<p>Don't forget to configure whatever application you choose for Hardware Acceleration
</p></li><li>
<h4>(For Australians) Install and donate to <a href="http://epgstream.net/">FreeEPG</a></h4>
<p>In Australia, the other choice is the more expensive <a href="http://www.icetv.com.au/">IceTV</a> service. Over-the-Air (OTA) EPG information is very basic/unreliable and really only gives you now/next, but with FreeEPG, you get at least 24 hours in advance, but more often 7 days in advance.

</p></li><li>
<h4>Install Video Browser</h4>
<p>It rocks so much, it's not optional. Okay, I'm a little biased being on the development team, but WMC's management of video hasn't evolved to keep up new media centers like <a href="http://www.boxee.tv/">Boxee</a> or XBMC. Specifically, those other players can get/reader metadata, display posters, and generally organise better.</p>
<div style="text-align: center;"><a rel="lightbox" href="http://images.theleagueofpaul.com/postimages//wmc-vid.jpg"><img style="border-width: 0px; display: inline;" title="wmc_vid" src="http://images.theleagueofpaul.com/postimages//wmc-vid-thumb.jpg" alt="wmc_vid" border="0" height="218" width="366"></a><br>
<em>Default video navigation in Media Center</em>
<p><em></em><a rel="lightbox" href="http://images.theleagueofpaul.com/postimages//vb-vid1.jpg"><img style="border-width: 0px; display: inline;" title="vb_vid1" src="http://images.theleagueofpaul.com/postimages//vb-vid1-thumb.jpg" alt="vb_vid1" border="0" height="219" width="366"></a><br>
<em>Video Browser, same view but with the help of posters<br>
and metadata, its a lot more manageable</em>.</p>

<p><a rel="lightbox" href="http://images.theleagueofpaul.com/postimages//vb-vid2.jpg"><img style="border-width: 0px; display: inline;" title="vb_vid2" src="http://images.theleagueofpaul.com/postimages//vb-vid2-thumb.jpg" alt="vb_vid2" border="0" height="218" width="366"></a><br>
<em>Video Browser, alternative view, yay for the power of choice </em></p>
<p><em></em>(click any image for a larger view)</p></div>
<p>Video Brower and Open Media Library are two addins (I'm sure there are more) that give a lot more control of managing videos in Media Center.
</p></li><li>
<h4>Run Media Center</h4>
<ol>
<li>Use the wizard, configure TV tuner/EPG information ("The Guide")</li>
<li>Add folders to monitor</li>
<li>Tasks -&gt; Settings -&gt; General -&gt; Startup &amp; Window Behaviour: Set to start WMC when Windows start</li>

</ol>
</li>
<li>
<h4>Power Settings</h4>
<p>Control Panel -&gt; Power Options-&gt; Change Power Settings</p>
<ol>
<li>Set the computer to go to sleep after however long you like, I have it set to 30minutes.</li>
<li>(Still in the Power Options) Advanced -&gt; Sleep -&gt; Allow Hybrid Sleep -&gt; Off<strong><br>

Unless you set this to off your Media Center will not send itself to sleep</strong></li>
<li><img style="border-width: 0px; display: block; float: none; margin-left: auto; margin-right: auto;" title="poweroptions" src="http://images.theleagueofpaul.com/postimages//poweroptions.jpg" alt="poweroptions" border="0" height="346" width="327">Additional Settings -&gt; Require a password on wakeup -&gt; Setting: <strong>No<br>
</strong>Setting this to no means you don't have to click to log back into your media center after waking it up</li>
<li>Download and install <a href="http://slicksolutions.eu/mst.shtml">MCE Standby Tool</a> (MST)As stupid as it sounds, Media Center won't idle to sleep unless it is at the main menu.<br>
With the help of the <a href="http://slicksolutions.eu/mst.shtml">MST</a>, you can set a whole bunch of options to make your HTPC as energy efficient as possible.</li>

</ol>
</li>
<li>
<h4>(Optional) These are somewhat less recommended and if you're still having problems with your HTPC turning on/off when you don't want it to without reason.</h4>
<ol>
<li>Windows Updates -&gt; Change Settings -&gt; Set to manual so it doesn't automagically install OR download.<br>
This does mean you <em>should</em> from time to time install</li>

<li>In Media Center -&gt; Tasks -&gt; Settings -&gt; General -&gt; Optimisation -&gt; turn off/disable optimisation</li>
<li>Start -&gt; All Programs -&gt; Accessories -&gt; System Tools -&gt; Disk Defragmenter-&gt; Untick 'Run on a schedule (recommended)</li>

</ol>
</li>
<li>
<h4>(Optional) Cleaning up MP3/WMA Metadata</h4>
<p>Media Center (and most music players these days, be it portable players like an iPod or Windows Media Player) use metadata to identify media these days, and in the case of music, it's ID3 tags. You can use Windows Media Player (although I strongly recommended <strong>against</strong> it) or iTunes to manage metadata, but if you prefer to more control, <a href="http://www.mp3tag.de/en/">Mp3tag</a> or <a href="http://www.softpointer.com/tr.htm">Tag&amp;Rename</a> (better but not free) are great tools for editing a host of audio formats (including but not limited to, MP3, AAC, WMA, FLAC, OGG).</p>

<p>To batch download album art, it's hard to go past <a href="http://www.hydrogenaudio.org/forums/index.php?showtopic=57392">Album Art Downloader</a>
</p></li></ol>
<p><strong>Still got problems? </strong><br>
There are two fantastic community sites that may be able to help you out, <a href="http://thegreenbutton.com/">The Green Button</a> and <a href="http://xpmediacenter.com.au/">XPMediaCenter</a>.</p>
