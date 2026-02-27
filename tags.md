---
layout: default
title: Tags
permalink: /tags/
---

<section class="hero">
  <h1>Tags</h1>
  <p>Browse posts by tag.</p>
</section>

<div class="card" style="margin-top: 18px;">
  {% assign tags = site.tags | sort %}
  {% for tag in tags %}
    <h2 id="{{ tag[0] }}">#{{ tag[0] }}</h2>
    <ul class="post-list">
      {% for post in tag[1] %}
        <li>
          <a class="post-title-link" href="{{ post.url | relative_url }}">{{ post.title }}</a>
          <p class="post-excerpt">{{ post.excerpt | strip_html | truncate: 200 }}</p>
        </li>
      {% endfor %}
    </ul>
    <hr class="divider" />
  {% endfor %}
</div>
