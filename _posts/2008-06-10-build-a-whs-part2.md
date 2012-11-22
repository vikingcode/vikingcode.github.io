--- 
layout: page
title: "Build a Windows Home Server: Case and PSU"
description: "Build a Windows Home Server: Case and PSU"
funnelweb_id: 13
date: 2008-06-10 14:00:00 +10:00
tags: "buildingwhs case whs "
comments: true
---
<img src="http://images.theleagueofpaul.com/postimages//windows-home-server-logo.png" alt=""></p>
<p>It's been a month since <a href="/build-a-whs-part1.html">part one (CPU+Motherboard),</a> and I must admit the comments didn't help me choose at all! Why? The comments didn't lean one way or another.</p>
<p>I've decided in the end to go with an Intel E1200 and <a href="http://tw.giga-byte.com/Products/Motherboard/Products_Overview.aspx?ClassValue=Motherboard&amp;ProductID=2693&amp;ProductName=GA-G31M-S2L">Gigabyte GA-G31M-S2L</a>. Despite renewed searching for AMD power usage including the Semprons, the E1200 still seemed to have the edge <span style="font-size: xx-small;">(especially when you factor in pricing and availability)</span>. The difference in power at this end of the spectrum is pretty low - more power will be wasted by an inefficient PSU than an inefficient low end CPU.</p>
<p>This means going with the current market prices, the CPU is $50 and motherboard $70, meaning there is a healthy $480 in the budget for case, power supply, hard drive(s) and any third party cooling required.</p>
<h3>My requirements</h3>

<p>The ideal case would be a tiny unnoticeable case, yet somehow like the <a href="http://en.wikipedia.org/wiki/Tardis">Tardis</a> able to store 50 hard drives; and the ideal PSU would be 100% efficient, making no noise at all.&nbsp; Neither are going to happen, but what's the next best thing?</p>
<p>Since I've chosen to go with a motherboard with four hard drive ports, I want the case to be able to hold four hard drives. Optical drive really isn't a concern, because (with any luck) I'll only be installing WHS once.</p>
<h3>Meet the Contenders</h3>
<p>The reason I've chosen to combine both cases and power supplies is that although you can get some awesome looking cases, or some with fantastic functionality, those cases tend to go for AUD$200+, and then to get a "decent" <span style="font-size: xx-small;">(and by decent, I mean quiet and won't burn out once you overload it by plugging in three hard drives, not 1kW)</span> power supply will set you back another AUD$100-&gt;$150.</p>

<p>This system even on load and with 4 drives, will struggle to make it to 100w <span style="font-size: xx-small;">(<span style="text-decoration: underline;">actual</span> <span style="text-decoration: underline;">draw</span>, before factoring in power supply efficiency, would be about </span><a href="http://www.digit-life.com/articles2/storage/hddpower.html"><span style="font-size: xx-small;">~10w average per HDD</span></a><span style="font-size: xx-small;">, with a top of 40w for motherboard and cpu combined)</span>. Power supplies are less efficient at very low loads, so the closer the PSU's maximum wattage is to the actual/projected draw, the higher the efficiency should be. On the PSU front its very hard to find anything below 300w that claim to meet "<a href="http://www.80plus.org/">80PLUS</a>" standards, with Seasonic making a hard to find <span style="font-size: xx-small;">(in Australia)</span> 300w model, Zalman producing a 360w. Thermaltake have an interesting 5.25? bay model which puts out 270w.</p>
<table border="0" cellpadding="5" cellspacing="5" width="706">
<tbody>

<tr>
<td valign="top" width="170"><a href="http://images.theleagueofpaul.com/postimages//media.jpg"><img style="border-width: 0px;" src="http://images.theleagueofpaul.com/postimages//media-thumb.jpg" alt="media" border="0" height="106" width="174"></a></td>
<td valign="top" width="512">That was until I found the <a href="http://www.mini-box.com/"><strong>picoPSU</strong></a>, which was looking like a real winner...until I realised it really only has one or two hard drive power connectors!This thing is really tiny and efficient though, <a href="http://www.silentpcreview.com/article601-page1.html">"so tiny that 70 picoPSUs would fit inside the casing of a normal ATX power supply"</a>!</td>
</tr>
<tr>
<td valign="top" width="170"><a href="http://images.theleagueofpaul.com/postimages//nsk1300-q.jpg"><img style="border-width: 0px;" src="http://images.theleagueofpaul.com/postimages//nsk1300-q-thumb.jpg" alt="nsk1300_q" border="0" height="146" width="160"> </a><a href="http://images.theleagueofpaul.com/postimages//nsk1300-q.jpg"><img style="border-width: 0px;" src="http://images.theleagueofpaul.com/postimages//nsk4400-q-thumb.jpg" alt="NSK4400_q" border="0" height="160" width="160"></a></td>
<td valign="top" width="512">To move onto something with more connectors, we're looking at the standard size ATX PSU. The <strong>Antec EarthWatts</strong> line of power supplies are cheap, low noise, fairly efficient, and have a low wattage. I ended up with a 380w EarthWatts PSU in my HTPC build.The <strong>"New Solution" (NSK)</strong> range of cases from Antec provide pretty good price/performance, and all come with an Earthwatts PSU. The <a href="http://www.antec.com/us/productDetails.php?ProdID=91380">NSK1380</a> <span style="font-size: xx-small;">(350w, ~AUD$120)</span>, <a href="http://www.antec.com/us/productDetails.php?ProdID=93480">NSK3480</a> <span style="font-size: xx-small;">(380w, ~AUD$120)</span>, <a href="http://www.antec.com/us/productDetails.php?ProdID=94482">NSK4480</a> <span style="font-size: xx-small;">(380w, ~AUD$100)</span> and <a href="http://www.antec.com/us/productDetails.php?ProdID=96580">NSK6580</a> <span style="font-size: xx-small;">(430w, ~AUD$135)</span> all look like real contenders to me (the NSK2480 is what was used in my HTPC).

<p>They all share roughly the same design - just larger than the model number below it, except for the NSK1380 which is a cube form.</p>
<p><span style="font-size: xx-small;">(There is also the <a href="http://www.antec.com/ec/productDetails.php?ProdID=00400"><span style="color: rgb(0, 0, 0);">NSK4000</span></a> and <a href="http://www.antec.com/ec/productDetails.php?ProdID=00600"><span style="color: rgb(0, 0, 0);">NSK6000</span></a> which appear to be the NSK4480 and NSK6580 chassis respectively, sans power supply. The damage for the NSK4000 is only AUD$69 however, meaning it could be more suitable to be combined with the picoPSU.)</span></p></td>
</tr>
<tr>
<td valign="top" width="170"><a href="http://images.theleagueofpaul.com/postimages//file1185764842437.jpg"><img style="border-width: 0px;" src="http://images.theleagueofpaul.com/postimages//file1185764842437-thumb.jpg" alt="file1185764842437" border="0" height="157" width="174"></a></td>
<td valign="top" width="512">I must admit CoolerMaster, while a well and truely established giant in the PC Case industry, isn't one I'd normally choose from "low end" cases. I suppose it helps that this case is $200 without a PSU, meaning it is over budget and definitely not a low end case. The <a href="http://www.coolermaster.com/products/product.php?language=en&amp;act=detail&amp;tbcate=17&amp;id=6">iTower 930</a> looks like your standard beige (make that silver and or black these days!) ATX case with one "small" difference - 4 hot-swappable SATA drives. It includes little caddies to put your drives in, then you just slot them into the computer.While not a new idea, being SATA means not only do you not have to open the case to add a drive, you don't have to even shut down the machine!

<p>There are other advantages too, the iTower 930 is made from mostly steel, meaning it <em>should</em> produce less vibration than cases with a majority of aluminium. Whether or not that's the case is another matter.</p></td>
</tr>
</tbody>
</table>
<h3>Decision Time</h3>
<p>Sure, the iTower is probably the most "featuresome" case on this list but coming in at ~AUD$200 seriously hampers the rest of the budget, especially when you consider it still needs a power supply. Can the iTower compete with the cheaper NSK series (with or without the EarthWatts PSU)? Is the Antec EarthWatts efficient enough to knock out a picoPSU? Can the picoPSU safely be fitted with adapters for more drives? Have I left anything off my list?</p>
