--- 
layout: page
title: Building my HTPC (Part III)
description: This is an old post migrated from Wordpress, links may not function correctly. Building my HTPC, Part 3
funnelweb_id: 8
date: 2008-02-24 13:00:00 +11:00
tags: "buildinghtpc hardware "
comments: true
---
Despite Bluray defeating HD-DVD, I just bought myself a HD-DVD drive. Why? Well, a SATA (or IDE) Bluray drive will set you back over AUD$200 currently, and I just picked up the Xbox 360 HD-DVD Addon for a mere $48. That $48 also includes King Kong on HD-DVD!

>"But you're buying defunct technology!"

Yeah, that's true, but I'm picking up a whole heap of movies for dirt cheap - not just cheap for HD content, but cheaper than buying the 'normal' DVD versions. How much cheaper? Well, EzyDVD are (were?) running a HD-DVD clearance sale:

>     Title 	HD DVD Price 	DVD Price
>     Chronicles Of Riddick, The - Director's Cut (HD DVD) 	$9.92 	$38.99
>     Chronicles Of Riddick, The - Pitch Black 	$9.92 	$19.99
>     Heroes - Season 1 (7 Disc Box Set) 	$29.83 	$95.99
>     Shaun Of The Dead 	$9.92 	$12.98
>     Terminator 2 - Judgment Day 	$9.92 	$23.99
>     Total 	$69.51 	$191.94

(yes, I realise most of these can also be bought in packs with other movies, so the savings could actually be much lower, but I'm trying to illustrate a point)

![xbox_hddvd][1]

So, what are the changes to the HTPC? PowerDVD Ultra is required for HD-DVD playback (UltraDVD would also work I guess, but I didn't have that on hand).

With my particular HTPC, I had to make sure that PowerDVD's options had hardware acceleration disabled, otherwise playback was choppy. I suspect that a lot of other HTPCs with integrated video probably face the same problem, enabling hardware acceleration actually hurts performance because too much of the processing is offloaded to the video chipset, and the video chipset is actually slower than doing it all on CPU.


  [1]: http://images.theleagueofpaul.com/postimages//xbox-hddvd.jpg
