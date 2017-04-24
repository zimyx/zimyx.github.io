---
layout: page
permalink: /sayings/
title:  "自说"
author_profile: true
---
  说说那些看到的（see） 听到的（listen） 嗅到的（smell） 尝到的（taste） 感到的（feeling） 想到的 （idea）

<ul>
{% for post in site.categories.sayings %}
  {% capture y %}{{post.date | date:"%Y"}}{% endcapture %}
  {% if year != y %}
    {% assign year = y %}
    <h2>{{ post.date | date: '%Y' }}</h2> 
  {% endif %}
  <li>
	<h4><span>{{ post.date | date:"%Y-%m-%d" }}</span>&raquo;
	<a href="{{ post.url }}" title="{{ post.title }}">{{ post.title }}</a></h4>
  </li> 
{% endfor %}
</ul>