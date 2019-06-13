---
layout: default
title: "我"
---

{% for c in site.data.me %}
  {{ c.name }}: <a href="{{ c.url }}"> {{ c.nickname }}</a>
{% endfor %}
