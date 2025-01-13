---
layout: page
title: News & Updates
permalink: /news/
---
## Latest News

{% for post in site.posts %}
  <article class="post-preview">
    <h3><a href="{{ post.url }}">{{ post.title }}</a></h3>
    <p class="post-meta">{{ post.date | date: "%B %d, %Y" }}</p>
    {{ post.excerpt }}
  </article>
{% endfor %}
