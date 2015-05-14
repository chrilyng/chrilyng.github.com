---
layout: page
title: Yet another developer blog 
tagline: Keeping myself updated on software trends
---
{% include JB/setup %}

## About me

I have a broad interest in most parts of software development from apps or web to databases and my experience is mostly with systems integration.

Developing software is engaging due to the quick feedback loop and the endless possibilites of realizing ideas.

I made this blog to keep track of my different experiments with trends in software libraries and frameworks.

### Profiles

* [Github](https://github.com/chrilyng/)

* [Bitbucket](https://bitbucket.org/chrilyng/)

* [LinkedIn](https://dk.linkedin.com/pub/christian-lyngbye/19/584/424)

### Contact
If you would like to contact me, send an email to (replacing AT): code AT siit.dk

[PGP public key](/assets/pubkey.txt)


## Posts

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>



