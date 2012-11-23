---
layout: page
title: Fixing XBox 360 "S" PSU noise
permalink: xboxfan.html
date: 2012-11-23
---

> This goes without saying, but this will void your warranty and you do so at your own risk.

##The Problem
Launched in 2010, the "S" model of the XBox 360 was much slimmer, a bit cooler and quieter. Kinda. The console unit itself is very quiet, particularly with the introduction to install games to the internal hard drive rather than spinning a DVD, but unfortunately the power supply didn't get such loving treatment in the overhaul.

Earlier in the year, I bought a second power supply, thinking that mine had just become worse and worse with time, but even the new unit had the same noise. The source of the noise in the PSU is a *tiny* 40mm (width/height) x 7.5mm (depth) fan. By contrast, a "standard" PC fans are 20-30mm deep, and range from 80mm up to 140mm. The tiny fan might have faired 'ok' if it wasn't hampered by several layers of plastic blocking a clear flow from the fan to the components, or if it had a sane fin design.

![](/images/postimages/WP_000231.jpg)

##The Fix
The only solution? Replace the fan. Now, I'm not the [first](http://forums.xbox-scene.com/index.php?showtopic=727454) to do this sort of thing, but others seem to have used 60mm for an easier fit. 

###The Process
**Opening up the power supply**  
To open up the power supply, you'll need a T10H screw driver - thats a Torx Security bit, aka hollow star. You'll also need a Phillips head once you get into the PSUto remove other components. The four Torx bits are on the underside of the PSU, underneath the rubber feet. You'll need to cut/pull those out, then some plastic tabs covering up the actual screws.

**Gutting**  
Once you've opened up the PSU, to completely separated the two halves of the PSU casing, unplug the fan. Try not to destroy the fans cable, but it isn't the end of the world if you do. Anything that can be unscrewed from the top half, should be, as well as the two oddly placed metal strips. You'll be left with a myriad of plastic and metal which can all be chucked out. Keep the fan, since you'll need its power cable.

**The Fan**  
The only [60mm fan](http://www.pccasegear.com/index.php?main_page=product_info&cPath=9_507&products_id=7762) I could find online spin at a less than stellar (for noise) 3500RPM. Instead, I used a Nexus Real Silent 80mm fan, which at 12vdc spins at a much lower 1500rpm, produces 17.6dBa (vs 23dBa of the 60mm) and pushes 20.2CFM (vs 18CFM).     

If you checked out the Xbox-Scene mod, you'll notice the 60mm fits nearly perfectly on top of the PSU, so a circle just needed to be cut out for it to work. With 80mm, mounting it like that would have resulted in a lot of air being blown around the PSU, rather than into it. Instead, I cut a square into the PSU casing, so that the fan is slotted into it. This means the 80mm actually fit in a bit lower than the 60mm, and is remarkably secure despite no adhesives (I do plan to add some glue down the track)

**Cutting**  
The easiest way is with a Dremel, I'm sure other cutting implements would work too. Measure whatever fan you're going to be using - in my case, 80mm - and tape off a section to be cut. Tape is a great way to mark the plastic that won't easily be covered by the fine dust that a Dremel produces when cutting plastic.
 
![](images/postimages/IMG_9510.jpg)

![](images/postimages/IMG_9512.jpg)

**Wiring**  
Typical PC fans are 3 pin/wire while the PSU fan is 2 pin. On the PCB, you'll note just 2 pins as well and the spacing is *slightly* smaller than a standard PC connector. This presents a problem which is easily solved with a bit of surgery. 

* Cut the connector off from the PSU fan
* Cut the connector off from your PC fan
* Strip back the red/black wires on the PC fan, and strip back the two wires from the PSU fan.
* Twist the red with red, black with black. 
* You can solder or just tape each join separately.

Once you're done, the wire can be fed through the LED diffuser ("power light") next to the power cable.

![](/images/postimages/WP_000271.jpg)