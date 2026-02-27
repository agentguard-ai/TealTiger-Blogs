---
layout: default
title: TealTiger Blog
---

<section class="hero">
  <h1>Engineering notes for agentic AI security</h1>
  <p>
    TealTiger is a developer-first runtime governance SDK for agentic AI. This blog shares practical patterns,
    reference architectures, and hard lessons—from zero trust to blast-radius control.
  </p>
</section>

<div class="grid">
  <section class="card">
    <h2>Latest posts</h2>

    <ul class="post-list">
      {% for post in site.posts limit: 8 %}
        <li>
          <a class="post-title-link" href="{{ post.url | relative_url }}">{{ post.title }}</a>
          <div class="post-meta" style="margin-top: 6px;">
            <time datetime="{{ post.date | date_to_xmlschema }}">{{ post.date | date: '%b %d, %Y' }}</time>
            {% if post.tags %}
              <span class="dot">·</span>
              <span>
                {% for tag in post.tags limit: 3 %}
                  <span class="badge">#{{ tag }}</span>
                {% endfor %}
              </span>
            {% endif %}
          </div>
          {% if post.excerpt %}
            <p class="post-excerpt">{{ post.excerpt | strip_html | truncate: 200 }}</p>
          {% endif %}
        </li>
      {% endfor %}
    </ul>
  </section>

  <aside class="card">
    <h2>Start here</h2>
    <p class="muted" style="line-height: 1.6;">
      Looking for the contract truth? Head to the docs.
    </p>
    <p>
      <a class="button" href="{{ site.docs_base }}">Open TealTiger Docs</a>
    </p>

    <hr class="divider" />

    <h2>Subscribe</h2>
    <p class="muted" style="line-height: 1.6;">Get new posts via RSS.</p>
    <p>
      <a class="nav-link" href="{{ '/feed.xml' | relative_url }}">RSS feed →</a>
    </p>

    <hr class="divider" />

    <h2>Cross-links</h2>
    <p class="muted" style="line-height: 1.6;">
      Each post includes a “Related docs” section linking to the exact page in
      <a href="{{ site.docs_base }}">docs.tealtiger.ai</a>.
    </p>
  </aside>
</div>
``
