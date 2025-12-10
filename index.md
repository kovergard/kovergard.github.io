---
title: "Welcome to the blog"
layout: default
---

# This is index.md

And some content from index

## List of blog posts

<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>
