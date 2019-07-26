---
layout: page
title: All articles are here
titlebar: archives
subtitle: <span class="mega-octicon octicon-calendar"></span>&nbsp;&nbsp;专题系列： &nbsp;&nbsp; <a href ="http://hunzino1.github.io/stack.html"><font color="#1A0DAB">技术栈</font></a>&nbsp;&nbsp; <a href ="http://hunzino1.github.io/life.html"><font color="#EB9439">故事</font></a>&nbsp;&nbsp; <a href ="http://hunzino1.github.io/algorithm-tree.html"><font color="#23527C">算法</font></a>&nbsp;&nbsp; <a href ="http://hunzino1.github.io/rails_stack.html"><font color="#1E90FF">rails技术栈</font></a> &nbsp;&nbsp; <a href ="http://hunzino1.github.io/psychology.html"><font color="#1E90FF">心理学</font></a>menu: archives
css: ['blog-page.css']
permalink: /archives.html
---

<ul class="archives-list">
  {% for post in site.posts %}

    {% unless post.next %}
      <h3>{{ post.date | date: '%Y' }}</h3>
    {% else %}
      {% capture year %}{{ post.date | date: '%Y' }}{% endcapture %}
      {% capture nyear %}{{ post.next.date | date: '%Y' }}{% endcapture %}
      {% if year != nyear %}
        <h3>{{ post.date | date: '%Y' }}</h3>
      {% endif %}
    {% endunless %}

    <li><span>{{ post.date | date:'%m-%d' }}</span> <a href="{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>
