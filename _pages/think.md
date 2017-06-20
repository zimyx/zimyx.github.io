---
layout: page
permalink: /think/
title:  "自悟"
author_profile: true
---
   问题导向 结果检验
现状分析（对标） -> 改进措施（方法） 目标KPI（责任） 行动计划（执行）  跟进考核（检验）

制定 ：
宣贯 ：
执行 ： 
跟踪 ：
考核 ：


好的工作方法可以娱己娱人  

<div>

  {% capture written_year %}'None'{% endcapture %}

  {% for post in site.categories.think %}

    {% capture year %}{{ post.date | date: '%Y' }}{% endcapture %}
    {% if year != written_year %}
      <h2 id="{{ year | slugify }}" class="archive__subtitle">{{ year }}</h2>
      {% capture written_year %}{{ year }}{% endcapture %}
    {% endif %}
    {% include archive-single.html %}

  {% endfor %}

</div>