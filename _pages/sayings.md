---
layout: page
permalink: /sayings/
title:  "自说"
author_profile: true
excerpt: "A selection of things I've designed, illustrated, and developed."
---
{% if page.header.overlay_color or page.header.overlay_image or page.header.image %}
  {% include page__hero.html %}
{% elsif page.header.video.id and page.header.video.provider %}
  {% include page__hero_video.html %}
{% endif %}

{% if page.url != "/" and site.breadcrumbs %}
  {% unless paginator %}
    {% include breadcrumbs.html %}
  {% endunless %}
{% endif %}
 
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