---
layout: page
title: Tag Cloud
comment: false
---

{% comment%}
Here we generate all the tags.
{% endcomment%}

{% assign rawtags = "" %}
{% for item in site.texts %}
{% assign ttags = item.tags | join:'|' | append:'|' %}
{% assign rawtags = rawtags | append:ttags %}
{% endfor %}

{% assign rawtags = rawtags | split:'|' | sort %}

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

<h1 class="page-title">
  <a href="/blog">Blog</a> | {{ page.title }}
</h1>
<br/>

<div class="posts">
<p>
{% for tag in tags %}
<a href="#{{ tag | slugify }}" class="codinfox-tag-mark"> {{ tag }} </a> &nbsp;&nbsp;
{% endfor %}

{% for tag in tags %}
<h2 id="{{ tag | slugify }}">{{ tag }}</h2>
<ul class="codinfox-category-list">
  {% for item in site.texts %}
  {% if item.tags contains tag %}
  <li>
    <h3>
      <a href="{{ item.url }}">
        {{ item.title }}
        <small>{{ item.date | date_to_string }}</small>
      </a>
      {% for tag in item.tags %}
      <a class="codinfox-tag-mark" href="/blog/tag/#{{ tag | slugify }}">{{ tag }}</a>
      {% endfor %}
    </h3>
  </li>
  {% endif %}
  {% endfor %}
</ul>
{% endfor %}
