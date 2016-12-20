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
   <ul>
   {% for post in site.posts %}
     <li> <a href="{{ site.baseurl }}{{ post.url }}"> {{post.title}}</a></li>
   {% endfor %}
   </ul>
 </div>
</div>