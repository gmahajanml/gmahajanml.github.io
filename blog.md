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

## Complete List
<div class="row">
    <div class="col-md-12">
<ul>
   {% for post in site.posts %}
     <li> <a href="{{ site.baseurl }}{{ post.url }}"> {{post.title}}</a></li>
   {% endfor %}
   </ul>
</div>
</div>

## Categories
<ul class="tags">
{% for category in site.categories %}
{% assign c = category | first %}
{% assign posts = category | last %}
<li><a href="#{{c}}" data-toggle="collapse">{{c}} ({{ posts | size }})</a></li>
{% endfor %}
</ul>

{% for category in site.categories %}
{% assign c = category | first %}
{% assign posts = category | last %}
<div class="row">
    <div class="col-md-12">
        <div id="{{c}}" class="collapse">
        <h3>{{c}} ({{ posts | size }})</h3>
            <ul>
                {% for post in posts %}
                <li> <a href="{{ site.baseurl }}{{ post.url }}"> {{post.title}}</a></li>
                {% endfor %}
            </ul>
        </div>
    </div>
</div>
{% endfor %}

## Tags
<ul class="tags">
{% for tag in site.tags %}
{% assign t = tag | first %}
{% assign posts = tag | last %}
<li><a href="#{{t}}" data-toggle="collapse">{{t}} ({{ posts | size }})</a></li>
{% endfor %}
</ul>

{% for tag in site.tags %}
{% assign t = tag | first %}
{% assign posts = tag | last %}
<div class="row">
    <div class="col-md-12">
        <div id="{{t}}" class="collapse">
        <h3>{{t}} ({{ posts | size }})</h3>
            <ul>
                {% for post in posts %}
                <li> <a href="{{ site.baseurl }}{{ post.url }}"> {{post.title}}</a></li>
                {% endfor %}
            </ul>
        </div>
    </div>
</div>
{% endfor %}