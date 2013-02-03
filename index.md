---
layout: index
title: Blog
---
{% for post in site.posts %}
  <div class="post_preview">
     <a href="{{ post.url }}"><b>{{ post.title }}</b></a> (from {{ post.date | date_to_string }}) <br>
     {{ post.content | strip_html | truncatewords: 42 }}
  </div>
  </br>
{% endfor %}
