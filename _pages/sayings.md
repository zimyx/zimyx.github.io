---
layout: archive
permalink: /sayings/
title:  "Sayings"
author_profile: true
---
  唠叨些看到的（see） 听到的（listen） 嗅到的（smell） 尝到的（taste） 感到的（feeling） 想到的 （idea） 
<div>
  {% capture y %}'None'{% endcapture %}
  {% for post in site.categories.sayings %}
    {% capture year %}{{ post.date | date: '%Y' }}{% endcapture %}
    {% if year != y %}
      <h2 id="{{ year | slugify }}" class="archive__subtitle">{{ year }}</h2>
      {% capture y %}{{ year }}{% endcapture %}
    {% endif %}
    {% include archive-single.html %}
  {% endfor %}
</div>
