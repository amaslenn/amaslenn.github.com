---
title: Blog of amaslenn
---
{% for post in site.posts %}
  {% assign show = true %}

  {% for tag in post.tags %}
    {% if tag == 'hidden' %}
      {% assign show = false %}
    {% endif %}
  {% endfor %}

  {% if show %}
  <div style="padding-bottom:0.9em" class="post_preview">
     <div><a href="{{ post.url }}"><b>{{ post.title }}</b></a> (from {{ post.date | date_to_string }})</div>
     {{ post.content | strip_html | truncatewords: 42 }}
  </div>
  {% endif %}
{% endfor %}
