--- 
layout: page
title: Building my HTPC (Part II)
description: This is an old post migrated from Wordpress, links may not function correctly. Building my HTPC, Part 2
funnelweb_id: 7
date: 2008-02-16 13:00:00 +11:00
tags: "buildinghtpc hardware "
comments: true
---
The proposed HTPC went ahead, and the final list of parts was

Software

 - Vista Ultimate 32-bit with Service Pack 1 (didn't have the x64 install iso downloaded yet)
 - CoreAVC
 - FFDshow

Hardware

 - Intel Pentium E2160
 - Asus P5K-VM
 - 500gb Western Digital HDD
 - 2gb DDR-2 Kingston 6400 (800mhz) Kit
 - Antec NSK2480
 - Logitech Harmony 525

![alt text][1]

From my list of parts, the dedicated graphics card has been dropped (for now, its a money thing), and the CPU and motherboard were upgraded. The CPU was upgraded from an E2140 (1.6ghz) to a E2160 (1.8ghz) simply because that's all the store had in at the time.

Case
====

![2259191425_0b55371bc8_o][2]

The Antec NSK2480 is not designed as a HTPC case, but as a general desk case. The HTPC version of it is the overpriced Fusion 430 (aka Fusion V2), which comes with a 430w PSU (+50w), a VFD display and a 'stereo' looking volume knob - it really isn't worth the extra AUD$100 for it.

There are really only two problems I have with this case (and I'd imagine would apply to the Fusion 430 as well); it uses very bright blue LED's (which just require you to not plug in the HDD LED and separate power LED) and that there isn't a way to "stealth" the optical drive - granted it isn't a HTPC case so it doesn't "require" it, but given how nice the case looks it certainly wouldn't hurt.

Motherboard & CPU
=================

The CPU is a Conroe core, despite having the Pentium moniker instead of Core2Duo. The E2160 is a 65nm chip, clocked at 1.8ghz, with 1mb L2 cache and a 800mhz front side bus. The beauty is, its the cheapest dual core processor from Intel (that the store had in stock, with the E2140 being cheaper and slower) yet it is more than capable of decoding 1080p content.

The choice of motherboard was to opt for a better featured (specifically, included SPDIF out), more robust motherboard. Asus' top of the line uATX motherboard goes for ~$250, and the P5K-VM is pretty much the next step down, but only costs ~$150.

![mobo][3]

While the motherboard was more expensive than what I "needed", the cheaper ones available are generally regarded as 'okay' but not 'rock solid' if you attempt overclocking. By just ramping up the front side bus (FSB), still using the stock Heat Sink/Fan (HSF), I was able to get this CPU up to 2.94ghz. That's an overclock of a fairly impressive 1.14ghz! Somewhat unneeded for HTPC duties as it decodes 1080p perfectly well, but for gaming and other duties I may put this computer through later on in its life, certainly an added bonus out of a $83 CPU

Remote.
=======

Only a brief word on the remote, the Logitech Harmony 525. It is your 'run of the mill' universal remote capable of learning IR commands, but it also has USB connectivity to download IR commands from Logitech's database. I won't say the database is flawless, but it had an entry for Media Center, my TV (Bravia KDL40W3100), my speakers (no surprises on that one however, they are Logitech speakers that were hooked up to my computer - we still "need" to get an amp/standalone speakers) and even the air conditioner! On top of that, the 'activities' options are awesome - if I want to watch TV, it switches the TV on, the speakers on, and the speakers to the right input. Likewise, if I want to watch something on Media Center, it'll switch the TV on, Speakers On, Change TV Input, Change Speaker Input, and even go to 'My Videos' (if I had a MCE IR Receiver).

I will give Logitech huge credit for the packaging. It is a standard looking blister pack which would normally require scissors and much bleeding, but this pack had a 'tear away' section, where little perforations in the plastic meant no cutting (or bleeding) was needed. The contents of the packaging also included 4xAAA Duracell batteries as well as 4xAAA Duracell batteries already in the remote!

Performance
===========

Thanks to CoreAVC, it is actually possible to play back HD (1080p that is) material without a dedicated graphics card. This rig is capable of playing back H264 without any sync/stuttering issues. VC-1 plays back fine too, but every now and again it was as if it was dropping frames - I haven't had the time to properly research the subject - VC1 is not decoded by CoreAVC as far as I know - meaning I was using the stock Vista/WMP11 decoder.
Video 	Codec 	Min CPU Usage 	MAX CPU Usage
Transformers (1080p) 	h.264 	55% 	70%
The Matrix (1080p) 	h.264 	52% 	66%
Rush Hour 3 Trailer (1080p) 	h.264 	30% 	55%
Heroes (S1E1, 720p) 	VC-1 	42% 	60%

Thanks to the power saving features of the CPU, it kept dropping down to about 1.9ghz (despite being overclocked to 2.9ghz) when playing back 1080p material - it simply has way too much oompf!

The other major codec to try out is PowerDVD, which has hardware acceleration through PureVideo/UVD, however that does not apply in this situation of onboard graphics. If I was to purchase the graphics card I intend to, yes, it would make a significant difference, however if I was to use CoreAVC which is a purely software/cpu decoder...well...the numbers in the table above won't change.

Power DVD performance
---------------------

Video 	Codec 	Min CPU Usage 	MAX CPU Usage
Transformers (1080p) 	h.264 	46% 	86%
The Matrix (1080p) 	h.264 	64% 	88%
Rush Hour 3 Trailer (1080p) 	h.264 	50% 	70%
Heroes (S1E1, 720p) 	VC-1 	48% 	60%

It's fairly easy to see that CoreAVC uses a lot less of the CPU than PowerDVD does - as much as 22% in some sections! Throw in a ATI HD 2400 Pro for as low as AUD$35, and the tables would certainly be reversed.

One interesting thing to note is that although the only VC1 content in this test had a higher CPU usage under PowerDVD, it was a LOT smoother, and didn't suffer from the occasional stuttering. It was also much faster at flicking through the video.

Final words..
=============

![tvtvtv][4]

I'm extremely happy with the performance, looks, noise and cost of this computer. All up, just AUD$667 (including remote)!


  [1]: http://images.theleagueofpaul.com/postimages//2259968614-2ff6856bdd-b.jpg
  [2]: http://images.theleagueofpaul.com/postimages//2259191425-0b55371bc8-o.jpg
  [3]: http://images.theleagueofpaul.com/postimages//mobo.jpg
  [4]: http://images.theleagueofpaul.com/postimages//tvtvtv.jpg
