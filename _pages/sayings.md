---
layout: archive
permalink: /sayings/
title:  "Z说"
author_profile: true
---
## 唠叨些： 
### 听到的[声](/tags/#%E5%A3%B0){: .btn .btn--small .btn--success}  看到的[色](/tags/#%E8%89%B2){: .btn .btn--small .btn--warning}  吓到的[犬](/tags/#%E7%8A%AC){: .btn .btn--small .btn--danger}  逛吃的[马](/tags/#%E9%A9%AC){: .btn .btn--small .btn--info} 
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
