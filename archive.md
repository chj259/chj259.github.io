---
title: Post Archive
permalink: /archive/
---
<h1> {{ page.title | escape }}</h1>
<div>
{% for tag in site.tags %}
<h2> {{ tag[0] }}</h2>
<ul >
{% for post in tag[1] %}
<li>
<a href= "{{ post.url }}" style="font-size: 20px; color:#71bdbf;"> {{ post.date | date: "%B %Y" }} - {{ post.title }}</a>
</li>
{% endfor %}
</ul>
{% endfor %}
</div>
