---
layout: default
permalink: /slides/
---

<div class="slides">
  {% for slide in site.slides %}
    <article class="slide">

      <h1><a href="{{ site.baseurl }}{{ slide.url }}">{{ slide.title }}</a></h1>

      <a href="{{ site.baseurl }}{{ slide.url }}" class="read-more">Read More</a>
    </article>
  {% endfor %}
</div>
