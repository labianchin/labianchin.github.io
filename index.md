---
layout: page
title: Luís Armando Bianchin
tagline: 
---
{% include JB/setup %}

Software engineer, software developer consultant and computer scientist. Graduated from [Federal University of Rio Grande do Sul](http://ufrgs.br/) in 2012. Technical interests include programming contests and challenges, software engineering techniques, Functional Programming, DevOps, Artificial Intelligence, Machine Learning and Information Retrieval.

This blog is a replacement of my old wiki website [http://inf.ufrgrs.br/~labianchin](http://inf.ufrgrs.br/~labianchin). This space is intended to share some knowledge.

E-mail: me \[ at \] labianchin.me

Some posts of this blog:

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>


