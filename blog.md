---
layout: default
title: Blog
subtitle: ML related articles
permalink: /blog/
---
<div id="blog-description" class="row">
  <div class="col-md-12"> 
  <p>
   This blog has articles on Automated Reasoning and Measure Theory.
   </p>
 </div>
</div>

<div id="blog-posts" class="row">
  <div class="col-md-12"> 
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
  </div>
</div>