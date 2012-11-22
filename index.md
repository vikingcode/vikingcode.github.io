---
layout : layout
---
{% for post in site.posts limit:1%}
<h1><a href="{{post.url}}">{{ post.title }}</a></h1>
{{ post.content }}
{% endfor %}

<h3>Older posts</h3>
<ul id="archive">
{% for post in site.posts offset:1 %}
	<li>
		<h3><a href="{{post.url}}">{{ post.title }}</a></h3>
		<div class="date">{{post.date | date: "%d %B %Y"  }}</div>
	</li>
{% endfor %}
</ul>