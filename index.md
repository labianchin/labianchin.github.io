---
layout: page
title: Luís Armando Bianchin
tagline: 
---
{% include JB/setup %}

I am a computer scientist. I have graduate from [Federal University of Rio Grande do Sul](http://ufrgs.br/) in 2012. My technical interests include programming contents and challenges, software engineering techniques, Software as a Service (SaaS), Artificial Intelligence, Machine Learning and Information Retrieval.

I have created this blog as a replacement of my old wiki website [http://inf.ufrgrs.br/~labianchin](http://inf.ufrgrs.br/~labianchin). I intent to use this space to share some knowledge.

E-mail: luis.armandob [ a t ] gmail.com

Here are a list of posts of this blog:

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>


