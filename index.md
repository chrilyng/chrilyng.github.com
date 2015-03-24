---
layout: page
title: Yet another developer blog 
tagline: Keeping track of musings
---
{% include JB/setup %}

## About me

I have a broad interest in all aspects of software development from apps or web to databases but my strengths are mostly in systems integration.

Developing software is engaging due to the quick feedback loop and the endless possibilites of realizing ideas.

### Portfolio

* [Github profile](https://github.com/chrilyng/)

* [Bitbucket profile](https://bitbucket.org/chrilyng/)

## Posts

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>



