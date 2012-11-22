--- 
layout: page
title: "Building my HTPC (Part IV: AMD Style)"
description: This is an old post migrated from Wordpress, links may not function correctly. Building my HTPC, Part 4
funnelweb_id: 9
date: 2008-02-27 13:00:00 +11:00
tags: "buildinghtpc hardware "
comments: true
---
Well okay, technically not my HTPC, but my mothers. That's right, my mum has a HTPC. She saw our one, thought it was pretty darn neat and decided she needed one as well (how cool is my mum? :D)

Parts
=====

 - AMD X2 4200+
 - Western Digital 500gb SATA
 - Asus 1814BL DVD-RW
 - Asus M2A-VM HDMI
 - Antec NSK2480
 - D-Link DWA-150
 - Corsair 2gb (2x1gb) DDR2 5300 Kit

Total cost, AUD$709! (why is this more expensive than the previous one? includes optical drive, wireless card and media centre remote.)

This time I've gone for the AMD route. Why? Price. While its a very similar price to what I paid for my HTPC, it has many more features. AMD mATX motherboards seem to be much better featured at up to $120 less.

Take the Asus P5E-VM HDMI (LGA775 - Intel) and the M2A-VM HDMI (AM2 - AMD). Both have HDMI/VGA output (the M2A-VM has DVI as well), both have digital sound output. Both boast HD-DVD/Bluray playback over HDMI. Both have respectable chipsets from their respective CPU makers. That's about where the similarities end.

The M2A-VM HDMI comes in at a tiny $99, whereas the P5E-VM HDMI will set you back upwards of $200! You can actually do a little bit of gaming (although, I wouldn't recommend too much gaming..) on the M2A-VM's intergrated video, the ATI X1250, whereas the GMA X3500 of the Intel board can just be plain nasty. Likewise, the video acceleration, which is what a board of this size is mostly likely to be doing, is far superior from the X1250. From my own experiences, hardware acceleration with Intel onboard graphics actually hampers playback speeds!

I mean, don't get me wrong, the P5E-VM HDMI is a very robust board, with a lot of features. Just the price over a very similarly equipped AM2 board puts it to shame. I wonder if its cheaper to make AMD boards due to licensing fees, or if having the memory controller on die like AMD do cuts costs that much..

AMD/Asus M2A-VM HDMI/ATI notes/issues
=====================================

Despite perhaps being better value, the AMD HTPC gave me a lot more trouble than my Intel HTPC.

 - With 'incorrect' (default) LAN drivers, I was only getting 10mbps instead of 1gbps,
 - The latest video card drivers (8.2 at time of writing) conflict with SP1 so that DVD playback through WMP/WMC (but not VLC) is unwatchable, using the drivers on the CD that comes with the motherboard allows you to view DVD's however, those drivers that came with the motherboard instantly crash VMC when trying to view LiveTV...
      Catalyst 8.1 fixes the issue. Tore my hair out fixing that one.
 - The motherboard bios, by default, does not have power management enabled for the fan. Turn this one immediately, because without it, the fan on the heatsink can be freaking loud. That's the scientific measurement, it is 1.5 Freaking Loud Values (FLV). With Power Management on, it'll drop to 0.3->0.5 FLV's
 - Even though it has improved from the Socket 939 design, the mounting system for AMD's AM2 stock heatsinks is still pretty poor compared to Intel's push-pin method. That being said, you generally only need to do it once.

Final Notes
===========

The only other part I didn't mention was the Windows Media Center Remote/IR Receiver. I picked up one of them for myself, as well as one for mum. The Logitech Harmony 525 I bought works flawlessly with it, and it makes navigating media so much easier than using a keyboard/mouse.

Just like the Intel HTPC, this system plays back the 1080p content I have flawlessly, although my mother only has a 720p TV, so it isn't as critical.
