---
bg: "tag.jpg"
layout: page
permalink: /books/
title: "Books"
crawlertitle: "Book Posts by Genre"
summary: "Genres on my shelf"
active: books
---

{% comment %}
=======================
The following part extracts all the tags from your posts and sort tags, so that you do not need to manually collect your tags to a place.
=======================
{% endcomment %}
{% assign rawgenre = "" %}
{% for post in site.posts %}
	{% assign genres = post.genre | join:'|' | append:'|' %}
	{% assign rawgenre = rawgenre | append:genres %}
{% endfor %}
{% assign rawgenre = rawgenre | split:'|' | sort %}

{% comment %}
=======================
The following part removes dulpicated tags and invalid tags like blank tag.
=======================
{% endcomment %}
{% assign genres = "" %}
{% for genre in rawgenre %}
	{% if genre != "" %}
		{% if genres == "" %}
			{% assign genres = genre | split:'|' %}
		{% endif %}
		{% unless genres contains genre %}
			{% assign genres = genres | join:'|' | append:'|' | append:genre | split:'|' %}
		{% endunless %}
	{% endif %}
{% endfor %}





{% for g in genres %}

  <h2 class="category-key" id="{{ g | downcase }}">{{ g |capitalize }}</h2>

  <ul class="year">
    {% for p in site.posts %}
      {% if p.genre == g %}
        <li>
          {% if p.lastmod %}
            <a href="{{ p.url | relative_url}}">{{ p.title }}</a>
            <span class="date">{{ p.lastmod | date: "%m-%d-%Y"  }}</span>
          {% else %}
            <a href="{{ p.url | relative_url}}">{{ p.title }}</a>
            <span class="date">{{ p.date | date: "%m-%d-%Y"  }}</span>
          {% endif %}
        </li>
      {% endif %}
    {% endfor %}
  </ul>
{% endfor %}
