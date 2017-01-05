---
layout: default
title: Blog
subtitle: ML related articles
permalink: /blog/
---

<div class="row">
  <div class="col-md-12"> 
      <p>
       This blog has articles on Automated Reasoning and Measure Theory. Few selected articles:
   </p>
   <ul>
       {% for post in site.posts %}
       {% if post.starred %}
       <li> <a href="{{ site.baseurl }}{{ post.url }}"> {{post.title}} </a></li>
       {% endif %}
       {% endfor %}
   </ul>
</div>
</div>

## Categories
<div class="panel-group" id="category-list">
    <ul class="tags">
        {% for category in site.categories %}
        {% assign c = category | first %}
        {% assign posts = category | last %}
        <li><a href="#{{c | replace: ' ', '-'}}" data-parent="#category-list" data-toggle="collapse">{{c}}({{ posts | size }})</a></li>
        {% endfor %}
    </ul>

    {% for category in site.categories %}
    {% assign c = category | first %}
    {% assign posts = category | last %}
    <div class="panel borderless">
        <div id="{{c | replace: ' ', '-'}}" class="panel-collapse collapse">
            <h3>{{c}} ({{ posts | size }})</h3>
            <ul>
                {% for post in posts %}
                <li> <a href="{{ site.baseurl }}{{ post.url }}"> {{post.title}}</a></li>
                {% endfor %}
            </ul>
        </div>
    </div>
    {% endfor %}
</div>

## Tags
<div class="panel-group" id="tags-list">
    <ul class="tags">
        {% for tag in site.tags %}
        {% assign t = tag | first %}
        {% assign posts = tag | last %}
        <li><a href="#tag-{{t | replace: ' ', '-'}}" data-parent="#tags-list" data-toggle="collapse">{{t}}({{ posts | size }})</a></li>
        {% endfor %}
    </ul>

    {% for tag in site.tags %}
    {% assign t = tag | first %}
    {% assign posts = tag | last %}
    <div class="panel borderless">
        <div id="tag-{{t | replace: ' ', '-'}}" class="panel-collapse collapse">
            <h3>{{t}} ({{ posts | size }})</h3>
            <ul>
                {% for post in posts %}
                <li> <a href="{{ site.baseurl }}{{ post.url }}"> {{post.title}}</a></li>
                {% endfor %}
            </ul>
        </div>
    </div>
    {% endfor %}
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