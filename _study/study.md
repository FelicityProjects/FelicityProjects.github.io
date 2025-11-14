---
layout: default
title: "스터디"
permalink: /study/
---

<h1>스터디</h1>

<ul>
  {% for doc in site.study %}
    <li>
      <a href="{{ doc.url }}">{{ doc.title }}</a>
    </li>
  {% endfor %}
</ul>
