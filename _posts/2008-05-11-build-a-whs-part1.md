--- 
layout: page
title: "Build a Windows Home Server: CPU &amp; Motherboard Selection"
description: This is an old post migrated from Wordpress, links may not function correctly. Building my WHS
date: 2008-05-11 14:00:00 +10:00
tags: "buildingwhs cpu whs "
comments: true
---
After my HTPC building series of blog posts, a Channel8'er was disappointed that I didn't include others in the building process of it. This time I've decided to make a Windows Home Server box, this time including the community to help me choose the parts.

Unlike Channel 8's Max Builds a PC series, I cannot be giving this away (finances simply don't allow it), so its' purely for participation value. I'll try and round up some prizes (maybe Expression Studio 2? I'll have to see what restrictions the copy I'll be getting has), but can't promise anything.

Why build a server, based on WHS? WHS is easy. It's easy to setup, it's easy to use, it's easy to maintain. I have better things to be doing that learning the ins and outs of either Windows Server or Linux to perform the same functions that WHS does out of the box. Well, that and Nick has one, so I'm jealous.

Minimum system requirements
===========================

The following specifications are the minimum defined by Microsoft for Windows Home Server

 - 1 GHz Intel Pentium 3 (or equivalent) processor
 - 512 MB RAM
 - 80 GB internal hard drive as primary drive
 - 100 Mbit/s wired Ethernet

My requirements
===============

When building most "normal" computers (desktop use/work use/whatever), the two main factors are usually performance and price. The usual process is to get as much power in to as small a budget as possible.

While this remains true for my desired server, there are other characteristics that have equal or greater importance. For example, low power usage is very important due to the "always on" nature of a server, and because of the location the server will run, low noise is equally important.

In order of importance, those key factors are:

 - price
 - low noise
 - low power usage
 - performance

Price
=====

The budget for this project is AUD$600 (give or take $50) for case, power supply, hard drive(s), motherboard, CPU, ram and any additional cooling needed. This does not include the WHS license cost.

Meet today's Contenders
=======================

This post looks specifically at CPUs and Motherboards. Why both? Well, for the lowest power/noise solutions, often the CPU either only comes with a motherboard or is actually soldered in.

From a CPU point of view, my research based on pricing and/or power usage leads me to believe that the "real" contenders are:

 - From AMD's low power series, the BE X2 2350. These have the advantage of having very good performance per watt as well as being a standard AM2 socket CPU so that it can be used in any standard AM2 motherboard, but suffer from slightly higher price (over $100) and power usage compared to some of the other contenders.
 - From VIA, the C7 range of CPU's, which range from 1.1 to 1.7ghz, all come bundled with a motherboard. The main downside to VIA is the limitation to just two SATA ports, effectively limiting the server to (with technology available now) 2TB
 - From Intel, the yet unreleased "Atom" range of CPU's. With little known about the Atom apart from its low power usage, it is hard to know what socket it will use, let alone pricing or availability!
 - Again from Intel, the Celeron E1200, weighing in at a tiny AUD$55 currently, and like the BE X2 2350, is able to used in a standard (LGA755)/readily available motherboard.

Information

 - [Digit-Life's Pentium E2140/2160 And Athlon X2 BE-2350: Low-Power CPU Benchmarks (E2140 "beats" BE-2350, by having lower power usage)][1]
 - [XBit Lab's Dual Core Shootout (E1200 "wins", again by having lowest power usage)][2]

From the motherboard point of view, it depends on the CPU chosen.

 - For AMD, I actually don't know what motherboard would be 'worth while', since all I've looked at over the last few months has been Intel (gaming rig for my wife, or HTPC). That being said, I think any 690g based board, such as Asus' M2A-VM would be more than suffice, given gigabit Ethernet and 4xSATA controller capabilities of the chipset.
 - For VIA, a good combination seems to be the MM3500 (not actively promoting Pioneer Computers Australia, but they had a good product page), which has a 1.5ghz C7 CPU, gigabit Ethernet, but is limited to just two SATA ports.
 - For Intel's Atom, there isn't enough information to judge, but for the E1200, Gigabyte's G31M-S2L or Asus' P5KPL-VM both feature gigabit Ethernet, 4 SATA ports, on board video, etc. The mATX form factor also helps by not limiting our case selection down the track. These boards both come in at about AUD$75

Decision Time...
==============

What would you choose, for both CPU and motherboard, taking cost, power and features into consideration? Is limiting the server to 2TB (before buying a PCI/PCIe SATA controller) balanced out by low power usage of VIA, or do the 'big boys' from AMD and Intel win the day? Have I rounded up all the viable options?


  [1]: http://www.digit-life.com/articles2/cpu/intel-pentium-e2140-2160.html
  [2]: http://www.xbitlabs.com/articles/cpu/display/dualcore-shootout_8.html
