---
layout: default
title: Blog
permalink: /blog/
---

<div id="posts">
  {% for post in site.posts %}
  <div class="row">
    <article class="col-md-12 post">
      <h2>{{ post.title }}</h2>

      <div>
        {{ post.excerpt }}
      </div>
      <a href="{{ site.baseurl }}{{ post.url }}">Read More</a>
    </article>
  </div>
  <hr>
  {% endfor %}
</div>