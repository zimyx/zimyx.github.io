---
layout: archive
permalink: /quotations/
title:  "Z话"
author_profile: true
---
 ——自我提醒的话
<div>
  {% capture written_year %}'None'{% endcapture %}
  {% for post in site.categories.quotations %}
    {% capture year %}{{ post.date | date: '%Y' }}{% endcapture %}
    {% if year != written_year %}
      <h2 id="{{ year | slugify }}" class="archive__subtitle">{{ year }}</h2>
      {% capture written_year %}{{ year }}{% endcapture %}
    {% endif %}
    {% include archive-single.html %}
  {% endfor %}
</div>
