---
layout: page
title: Blog
permalink: /blog/
---

![Blog Banner: Kagoshima Coast](/assets/images/kagoshima.jpg)

---

## Welcome

Where I write about things that interest me. I hope they interest you, too.

{% if site.posts.size > 0 %}
<h3 class="post-list-heading">{{ page.list_title | default: "Posts" }}</h3>
<ul class="post-list">
	{% for post in site.posts %}
	<li>
		{% assign date_format = site.minima.date_format | default: "%b %-d, %Y" %}
		<span class="post-meta">{{ post.date | date: date_format }}</span>
		<h4>
			<a class="post-link" href="{{ post.url | relative_url }}">{{ post.title | escape }}</a>
		</h4>
		{% if site.show_excerpts %}
			{{ post.excerpt }}
		{% endif %}
	</li>
	{% endfor %}
</ul>

{% endif %}
### [Categories]({{ '/blog/categories/' | relative_url }})
<p class="rss-subscribe">subscribe <a href="{{ "/feed.xml" | relative_url }}">via RSS</a></p>
