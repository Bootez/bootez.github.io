---
layout: page
title: Chance Favors the Prepared Mind
tagline:
---
{% include JB/setup %}

<!-- Content preview -->
<ul >
    {% for post in site.posts limit 4 %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
        {{ post.content | strip_html | truncatewords:75}}<br>
            <a href="{{ post.url }}">Read more...</a><br><br>
    {% endfor %}
</ul>

<!-- Here's a "posts list". -->
<!--
{% for post in site.posts %}
  <hr>
  <h1>{{post.title}}</h1>  
  {{post.date|date: "%Y-%m-%d"}}

  {{post.description}}

  {% if post.figure %}
<a href="{{post.url}}"><img src="{{post.figure}}"/></a>
  {% endif %}

  [More...]({{post.url}})
{% endfor %}
-->
