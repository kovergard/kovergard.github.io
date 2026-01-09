---
layout: base
---

# Posts

<ul class="post-list">
{% for post in site.posts %}
<li>
  <hr>
  <h3><a class="post-link" href="{{ post.url }}">{{ post.title }}</a></h3>
  <span class="post-meta">{{ post.date | date: "%Y-%m-%d" }}
  {% if post.tags %}
    <br><span class="meta-label"><small><em>{{ post.tags | join: "</em> | <em>" }}</em></small></span>
  {% endif %}
  </span>
  <p>{{ post.content | strip_html | truncatewords: 50 }}</p>
</li>
{% endfor %}
</ul>