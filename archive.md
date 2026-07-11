---
layout: default
title: Archive
permalink: /archive/
---

<section class="intro">
  <h1>Archive</h1>
</section>

{% assign posts_by_year = site.posts | group_by_exp: "post", "post.date | date: '%Y'" %}
{% for year in posts_by_year %}
  <h2 class="archive-year">{{ year.name }}</h2>
  <ul class="post-list">
    {% for post in year.items %}
      <li>
        <time datetime="{{ post.date | date_to_xmlschema }}">{{ post.date | date: "%b %-d" }}</time>
        <div class="post-list-entry">
          <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
          {% if post.tags and post.tags.size > 0 %}
            {% assign primary_tag = post.tags | first %}
            <a class="topic-tag topic-tag-inline" href="{{ '/topics/' | relative_url }}#{{ primary_tag | slugify }}">{{ primary_tag }}</a>
          {% endif %}
        </div>
      </li>
    {% endfor %}
  </ul>
{% endfor %}
