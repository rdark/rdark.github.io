---
layout: default
title: Archive
permalink: /blog/
---

## Archive

<p class="rss-subscribe"><a href="{{ "/feed.xml" | prepend: site.baseurl }}">Feed</a></p>

{% for post in site.posts %}
  * {{ post.date | date_to_string }} &raquo; [ {{ post.title }} ]({{ post.url }})
{% endfor %}


