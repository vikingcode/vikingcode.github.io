--- 
layout: page
title: "Fixing Skypes crash-on-Windows-shutdown"
date: 2014-01-15
---

While Microsoft have turned Messenger off in favour of Skype, there still seems to be a lot to do before it can be considered not user-hostile. 

One of the infuriating issues I've run into over the last couple of years is it will decide it won't shut down cleanly when Windows/PC is shutdown. In fact it will halt the shutdown procedure until you manually dismiss the 'skype has crashed' message. If you leave it long enough, it actually cancels the shutdown process!

![](/images/postimages/skype_devil.jpg)

It turns out Skype has conflicts with sound cards. Many of them, but specifically Asus' Xonar soundcards. Sound works just fine through it (bidirectional), so I'm not sure what the exact issue is, perhaps it is one of timing. 

The solution is to go into Tools -> Options -> General/Sounds and disable the sound for ***I sign out*** event.

![](/images/postimages/skype_options.png)

Questionable programming and QC aside, now my computer shuts down when I want it to. Oh and don't give me that "try skype for windows 8" garbage. I like IM messages to be delivered in a relatively quick fashion, not hours/days/months later.