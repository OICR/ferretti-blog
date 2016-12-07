---
layout: tags
title: Sample Category
permalink: /blog/tags
header:
    version: small
    title: Software Engineering Blog
    image: header-logo-crop.png
    page: team
    icon: icon-blog
---


{% comment %}
=======================
The following part extracts all the tags from your posts and sort tags, so that you do not need to manually collect your tags to a place.
=======================
{% endcomment %}
{% assign rawtags = "" %}
{% for post in site.posts %}
  {% assign ttags = post.tags | join:'|' | append:'|' %}
  {% assign rawtags = rawtags | append:ttags %}
{% endfor %}
{% assign rawtags = rawtags | split:'|' | sort %}

{% comment %}
=======================
The following part removes dulpicated tags and invalid tags like blank tag.
=======================
{% endcomment %}
{% assign tags = "" %}
{% for tag in rawtags %}
  {% if tag != "" %}
    {% if tags == "" %}
      {% assign tags = tag | split:'|' %}
    {% endif %}
    {% unless tags contains tag %}
      {% assign tags = tags | join:'|' | append:'|' | append:tag | split:'|' %}
    {% endunless %}
  {% endif %}
{% endfor %}


{% comment %}
=======================
The purpose of this snippet is to list all your posts posted with a certain tag.
=======================
{% endcomment %}
{% for tag in tags %}
### {{ tag }}
  <div class="results">
    <ul class="no-bullet tags-list">
      {% for post in site.posts %}
        {% assign author = site.data.authors[post.author] %}
        {% if post.tags contains tag %}
        <li class="blog_teaser">
          {% assign authorsCount = post.authors | size %}
          {% assign authorCount = post.author | size %}
          <p class="post-author">
          {% if authorsCount > 0 %}            
              <strong>Posted By:
              {% for currentAuthor in post.authors %}
                {% assign author = site.data.authors[currentAuthor] %}
                <a href="{{ site.baseurl }}/blog/category/{{ currentAuthor }}">{{ author.name }}</a>{% unless forloop.last %}, {% endunless %}
              {% endfor %}
              on {{ post.date | date:'%b. %d, %Y' }}
              </strong>
          {% else if authorCount > 0 %}
            {% assign author = site.data.authors[post.author] %}
            <strong>Posted By: <a href="{{site.baseurl}}/blog/category/{{post.author}}">{{ author.name }}</a> on {{ post.date | date:'%b. %d, %Y' }}</strong>
          {% endif %}
          </p>
          <h4 class="post-title">
            <a href="{{ post.url }}">{{ post.title }}</a>
          </h4>
        </li>
        {% endif %}
      {% endfor %}
    </ul>
  </div>
{% endfor %}
