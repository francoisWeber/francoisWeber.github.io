---
layout: default
title: Accueil
---

# Blog de François Weber

Voici une liste de mes articles les plus récents :

<ul>
{% for article in site.articles %}
  <li>
    <a href="{{ article.url }}">{{ article.data.title }}</a>
  </li>
{% endfor %}
</ul>
