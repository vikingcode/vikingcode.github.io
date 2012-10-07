---
layout : layout
---
{% for post in site.posts limit:1%}
<h1><a href="{{post.url}}">{{ post.title }}</a></h1>
{{ post.content }}
{% endfor %}

<h3>Older posts</h3>
<ul>
{% for post in site.posts offset:1 %}
	<li>
		<a href="{{post.url}}">{{ post.title  |upcase }}</a>
		<div class="date">{{post.date | date: "%d %B"  }}</div>
	</li>
{% endfor %}
</ul>