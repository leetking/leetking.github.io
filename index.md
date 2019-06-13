---
layout: default
title: "发声练习"
---

<ul>
{% for page in site.posts %}
<li style="clear: right">
  <h3 style="margin: 0; display: inline-block"><a href="{{ page.url }}">{{ page.title }}</a></h3>
    {% if page.create_date %}
      <span style="float: right; height: 2em; line-height: 2em">
        {{ page.create_date | date: "%Y-%m-%d" }} / {{ page.modify_date | date: "%Y-%m-%d"}}
      </span>
    {% endif %}
</li>
{% endfor %}
</ul>
