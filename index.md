---
layout : layout
---
<h1>Hi, I'm Paul</h1>
<div id="about">A sometimes dev, woodworker and woodturner.</div>
<br />
<h2>Projects</h2>
<h3>Woodwork</h3>
Apart from building things, I also log them in video form - sometimes instructional - on <a href="http://www.youtube.com/playlist?list=PLIAY4op1UapjlW0WBJqvqzWir-bGCJerj"><img src="/images/youtube.png" title="YouTube" alt="YouTube" /></a>

<ul id="wwprojects">
<li><a href="http://www.flickr.com/photos/vikingcode/12281024344/"><img src="/images/wwprojects/huonpine.png" /></a></li>
<li><a href="http://www.flickr.com/photos/vikingcode/11825531614/"><img src="/images/wwprojects/redmalleepen.png" /></a></li>
<li><a href="http://www.flickr.com/photos/vikingcode/10831560306/"><img src="/images/wwprojects/jochop.png" /></a></li>
<li><a href="http://www.flickr.com/photos/vikingcode/10831712474/"><img src="/images/wwprojects/qldashbowl.png" /></a></li>
<li><a href="http://www.flickr.com/photos/vikingcode/11417267643/"><img src="/images/wwprojects/tealight.png" /></a></li>
<li><a href="http://www.flickr.com/photos/vikingcode/11459812524/"><img src="/images/wwprojects/jvotive.png" /></a></li>
<li><a href="http://www.flickr.com/photos/vikingcode/10974247284/"><img src="/images/wwprojects/jarrahpen.png" /></a></li>
<li><a href="http://www.flickr.com/photos/vikingcode/11448951175/"><img src="/images/wwprojects/quickunpick.png" /></a></li>

<li><a href="http://www.flickr.com/photos/vikingcode/10549480626/"><img src="/images/wwprojects/tasoaktable.png" /></a></li>
<li><a href="http://www.flickr.com/photos/vikingcode/10549539684/"><img src="/images/wwprojects/blackwoodtray.png" /></a></li>

</ul>

<div class="clear">&nbsp;</div>
<h3>Software</h3>
After awful experiences with overly opinionated, trolling-acceptable people, I no longer put my code out as open source.
However, some notable open-source projects I've worked are...

<div class="project">
	<div class="project_image"><img src="/images/markpad.png"></div>
	<div class="project_desc">
	 <strong><a href="https://github.com/Code52/DownmarkerWPF">MarkPad</a></strong><br />
	 And most of the Code52 projects, I was helping run the show after all!
	</div>
</div>
<div class="clear">&nbsp;</div>

<div class="project">
	<div class="project_image"><img src="/images/mahapps.metro.logo2.png"></div>
	<div class="project_desc">
	 <strong><a href="https://github.com/MahApps/MahApps.Metro">MahApps.Metro</a></strong><br />(founder)
	</div>
</div>

<div class="clear">&nbsp;</div>

<strong>Windows 8 apps</strong>
<div class="project">
	<div class="project_image"><img src="/images/pinorigin.png"></div>
	<div class="project_desc">
	 <strong><a href="/pinorigin/">PinOrigin</a></strong><br />
	 The best way to get all your games on display
	</div>
</div>
<div class="clear">&nbsp;</div>

<div class="project">
	<div class="project_image"><img src="/images/woodcalc.png"></div>
	<div class="project_desc">
	 <strong><a href="/woodcalc/">WoodCalc</a></strong><br />
	 The handy woodworkers companion.
	</div>
</div>
<div class="clear">&nbsp;</div>

<div class="project">
	<div class="project_image"><img src="/images/heimdall.png"></div>
	<div class="project_desc">
	 <strong><a href="/Heimdall/">Heimdall</a></strong><br />
	 Tired of logging in to MSDN for CDKeys or sifting through useless CSV files?<br/><br/>
Heimdall takes care of managing license keys by importing MSDN's exported XML and turning it into an easily searchable format.
	</div>
</div>
<div class="clear">&nbsp;</div>

<div class="project">
	<div class="project_image"><img src="/images/ragnarok.png"></div>
	<div class="project_desc">
	 <strong><a href="">Ragnarok</a></strong><br />
	</div>
</div>
<div class="clear">&nbsp;</div>

<div class="project">
	<div class="project_image"><img src="/images/metropolis.png"></div>
	<div class="project_desc">
	 <strong><a href="">Metropolis</a></strong><br />
	 Metropolis is able to leap NewsBlur accounts in a single bound!
	</div>
</div>
<div class="clear">&nbsp;</div>

<div class="project">
	<div class="project_image"><img src="/images/markpad.png"></div>
	<div class="project_desc">
	 <strong><a href="">MarkPad</a></strong><br />
	 Yes, two entries for Markpad. This one being the WinRT version.
	</div>
</div>
<div class="clear">&nbsp;</div>

<br />
<h2>Blog Posts</h2>
<ul id="archive">
{% for post in site.posts  %}
<li style="border-bottom: 1px solid #f5f5f5;">
	<div class="post_list_title"><a href="{{post.url}}">{{ post.title }}</a></div>
	<div class="post_list_date">	{{post.date | date: "%d %B %Y"  }}</div>
	<div style="clear:both"></div>
</li>
{% endfor %}
</ul>
