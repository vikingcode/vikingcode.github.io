--- 
layout: page
title: "Build a Windows Home Server: Building Hardware"
description: "Build a Windows Home Server: Building Hardware"
funnelweb_id: 24
date: 2008-08-12 14:00:00 +10:00
tags: "buildinghtpc hardware "
comments: true
---
<img src="http://images.theleagueofpaul.com/postimages//windows-home-server-logo.png" />
<h3 align="left">Parts list</h3>
<p>Changing technology has slightly altered my build list. With the launch of Intel's G45 motherboards, I decided to <a href="/review-gigabyte-ga-eg45m-ds2hintel-g45-chipset.html">upgrade my HTPC with a Gigabyte GA-EG45M-DS2H</a>, meaning I had a spare Asus P5K-VM which knocks the previously selected motherboard off the list.</p>
<p>I decided to go for a single 1TB drive - while it didn't represent the best price per gigabyte <font size="1">(currently 640gb HDD's do, something like 14c/GB vs 18c/GB)</font>, it wasn't overly expensive, gave me the convenience of a single drive with a lot of storage, and means I'll have more filespace by the time I exceed all my SATA ports (P5K-VM has four SATA ports).</p>


<table border="0" cellpadding="2" cellspacing="0" width="539">

<tbody>
<tr>
<td valign="top" width="130"><strong>Part</strong></td>
<td valign="top" width="249"><strong>Model</strong></td>
<td valign="top" width="158"><strong>Price (AUD)</strong></td>
</tr>
<tr>
<td valign="top" width="130"><strong>CPU</strong></td>
<td valign="top" width="249"><a href="http://processorfinder.intel.com/details.aspx?sSpec=SLAQW">Intel Celeron E1200</a></td>
<td valign="top" width="158">$45</td>
</tr>

<tr>
<td valign="top" width="130"><strong>Motherboard</strong></td>
<td valign="top" width="249"><a href="http://www.asus.com/products.aspx?modelmenu=2&amp;model=1690&amp;l1=3&amp;l2=11&amp;l3=542&amp;l4=0">Asus P5K-VM</a></td>
<td valign="top" width="158">~$120*</td>
</tr>
<tr>
<td valign="top" width="130"><strong>Case</strong></td>
<td valign="top" width="249"><a href="http://www.antec.com/us/productDetails.php?ProdID=96580">Antec NSK6850</a></td>
<td valign="top" width="158">$135</td>
</tr>
<tr>

<td valign="top" width="130"><strong>Power supply</strong></td>
<td valign="top" width="249"><a href="http://www.antec.com/us/productDetails.php?ProdID=27430">430w Earthwatts</a></td>
<td valign="top" width="158"><font size="1">Included with above case</font></td>
</tr>
<tr>
<td valign="top" width="130"><strong>Hard drive</strong></td>
<td valign="top" width="249"><a href="http://www.wdc.com/en/products/products.asp?driveid=336">Western Digital "Green Power" 1TB SATA</a></td>
<td valign="top" width="158">$182</td>
</tr>
<tr>
<td valign="top" width="130"><strong>RAM</strong></td>

<td valign="top" width="249"><a href="http://www.adata-group.com/en/product_show.php?ProductNo=AD2800U">1gb A-DATA DDR2-800</a></td>
<td valign="top" width="158">$25</td>
</tr>
<tr>
<td valign="top" width="130"><strong>CPU Cooler</strong></td>
<td valign="top" width="249"><a href="http://www.scythe-usa.com/product/cpu/032/scmnj1000_detail.html">Scythe Mini Ninja</a></td>
<td valign="top" width="158">$59</td>
</tr>
<tr>
<td valign="top" width="130">&nbsp;</td>
<td valign="top" width="249">&nbsp;</td>
<td valign="top" width="158">&nbsp;</td>

</tr>
<tr>
<td valign="top" width="130">&nbsp;</td>
<td valign="top" width="249"><strong>Total</strong></td>
<td valign="top" width="158"><strong>$566</strong></td>
</tr>
</tbody>
</table>
<p><font size="1">*Not the price I paid when I bought it, but current going price in Victoria</font></p>
<p>That's $34 under budget! Had I bought the NSK4480 ($95) and gone for the cheaper GA-G31M-S2L ($61) the total would have come down to a much more impressive $467 <font size="1">(or $133 under budget, even less if you don't mind more noise and could drop the Scythe CPU cooler)</font>. </p>
<p>I realise this doesn't include the cost of Windows Home Server (~$200) which bumps it up to $766 <font size="1">(/$667 if you chose the cheaper parts)</font>. At this stage a <a href="http://www.netgear.com/Products/Storage/ReadyNASDuo/RND2150.aspx">simple NAS unit</a> or something along the lines of an <a href="http://www.apple.com/au/timecapsule/">Apple Time Capsule</a> ($699 for 1TB) <em>may</em> seem like better value, but the thing to remember is this system can be easily upgraded to 4TB of storage by just putting more drives in, or up to 9TB (then you approach physical limitation of the NSK6850) by adding a SATA controller card! Most simple NAS units (at least, cheaper than this WHS build) have only one or two drive bays!</p>

<p>Oh, and for the record, I <em>was </em>after the NSK4480 instead of the NSK6850, but my local MSY were out of stock at the time. The differences come down to size and price, where the NSK4480 is smaller (7 vs 9 expansion bays) and costs less ($95 vs $135). </p>
<h3>The Cooler</h3>
<p align="center"><strong><a href="http://images.theleagueofpaul.com/postimages//img-8743-nocrop.jpg" rel="lightbox"><img style="border-width: 0px;" title="IMG_8743_NoCrop" alt="IMG_8743_NoCrop" src="http://images.theleagueofpaul.com/postimages//img-8743-nocrop-thumb.jpg" border="0" height="406" width="606"></a> </strong></p>
<p>You may be wondering why spend so much on cooling such a low end CPU, when the stock cooler barely breaks a sweat on it? Well, despite 'mini' being part of it's name, the Mini Ninja (above) is massive - it can take a lot of heat. Combine that with the slow spinning 120mm case fan included with the NSK6850, and temperatures have remained under 35c (mostly runs at 22c, while being under my desk in a low ventilated area) and the CPU cooler doesn't contribute to noise generation at all - it's hard to tell when this thing is operating!</p>
<h3>Power usage</h3>
<p>As I stated at the start of this series of posts, I wanted a system that had a low power draw. Measuring this system, it uses somewhere between 54 -&gt; 65w, sitting mostly on 55w. To be perfectly honest, I'm a <em>little</em> disappointed, but it turns out the <a href="http://www.silentpcreview.com/article684-page4.html">430w EarthWatts PSU is ~70% or less efficient</a> at these draw levels. With that in mind, that brings the realistic draw of the system down to 38 -&gt; 45w. When I've got time, I'll look at swapping in the 380w PSU from my HTPC, to see what the differences are.</p>

<p>At 60w (we'll say this is the average draw), that equates to <strong>$68/year to run.</strong> <font size="1">(365days * 24hours * 60watts * $0.1294<font color="#808080"> (cost per kWh Origin Energy charge me) </font>= $68.012)</font></p>
<p>Component choice helps keep the power levels low - the E1200 is a relatively low (processing) power CPU and as such uses less power than many other CPUs, <a href="http://www.silentpcreview.com/article786-page3.html">the hard drive consumes up to half the amount of power</a> of other drives at the expensive of throughput (the drive spins between 5400 and 7200RPM) and although negligible, a single RAM stick obviously consumes less than filling all four RAM slots.</p>
<h3>Gallery</h3>
<p align="center"><a href="http://images.theleagueofpaul.com/postimages//img-8744-nocrop.jpg" rel="lightbox"><img style="border-width: 0px;" title="IMG_8744_NoCrop" alt="IMG_8744_NoCrop" src="http://images.theleagueofpaul.com/postimages//img-8744-nocrop-thumb.jpg" border="0" height="139" width="206"></a> <a href="http://images.theleagueofpaul.com/postimages//img-8739-nocrop.jpg" rel="lightbox"><img style="border-width: 0px;" title="IMG_8739_NoCrop" alt="IMG_8739_NoCrop" src="http://images.theleagueofpaul.com/postimages//img-8739-nocrop-thumb.jpg" border="0" height="139" width="206"></a>&nbsp;<a href="http://images.theleagueofpaul.com/postimages//img-8741-nocrop.jpg" rel="lightbox"><img style="border-width: 0px;" title="IMG_8741_NoCrop" alt="IMG_8741_NoCrop" src="http://images.theleagueofpaul.com/postimages//img-8741-nocrop-thumb.jpg" border="0" height="139" width="206"></a> <a href="http://images.theleagueofpaul.com/postimages//img-8743-nocrop1.jpg" rel="lightbox"><img style="border-width: 0px;" title="IMG_8743_NoCrop" alt="IMG_8743_NoCrop" src="http://images.theleagueofpaul.com/postimages//img-8743-nocrop-thumb1.jpg" border="0" height="139" width="206"></a> <a href="http://images.theleagueofpaul.com/postimages//img-8781.jpg" rel="lightbox"><img style="border-width: 0px;" title="IMG_8781" alt="IMG_8781" src="http://images.theleagueofpaul.com/postimages//img-8781-thumb.jpg" border="0" height="139" width="196"></a>&nbsp;<a href="http://images.theleagueofpaul.com/postimages//img-8782.jpg" rel="lightbox"><img style="border-width: 0px;" title="IMG_8782" alt="IMG_8782" src="http://images.theleagueofpaul.com/postimages//img-8782-thumb.jpg" border="0" height="139" width="125"></a></p>

