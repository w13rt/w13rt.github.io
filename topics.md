---
layout: default
title: Topics
permalink: /topics/
---

<section class="intro topics-intro">
  <h1>Topics</h1>
  <p>Every post is listed below. Pick a topic to filter.</p>
</section>

{% assign topics = site.tags | sort %}
{% if topics.size > 0 %}
  <nav class="topic-controls" aria-label="Topics" id="topic-controls">
    <button class="topic-control" type="button" data-topic="" aria-pressed="true">All topics</button>
    {% for topic in topics %}
      <button class="topic-control" type="button" data-topic="{{ topic[0] | slugify }}" aria-pressed="false">{{ topic[0] }}</button>
    {% endfor %}
  </nav>

  <section class="topics-results" id="topics-results" aria-labelledby="topics-status">
    <p class="topics-status" id="topics-status" aria-live="polite"></p>
    <ul class="post-list" id="topic-posts">
      {% for post in site.posts %}
        <li data-topics="{% for tag in post.tags %}|{{ tag | slugify }}{% endfor %}|">
          <time datetime="{{ post.date | date_to_xmlschema }}">{{ post.date | date: "%b %-d, %Y" }}</time>
          <div class="post-list-entry">
            <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
            {% if post.tags and post.tags.size > 0 %}
              <ul class="post-tags" aria-label="Topics for {{ post.title }}">
                {% for tag in post.tags %}
                  <li><a class="topic-tag" href="{{ '/topics/' | relative_url }}#{{ tag | slugify }}">{{ tag }}</a></li>
                {% endfor %}
              </ul>
            {% endif %}
          </div>
        </li>
      {% endfor %}
    </ul>
  </section>

  <noscript>
    <section class="topics-fallback">
      <h2>All topics</h2>
      {% for topic in topics %}
        <h3>{{ topic[0] }}</h3>
        <ul class="post-list">
          {% for post in topic[1] %}
            <li>
              <time datetime="{{ post.date | date_to_xmlschema }}">{{ post.date | date: "%b %-d, %Y" }}</time>
              <div class="post-list-entry"><a href="{{ post.url | relative_url }}">{{ post.title }}</a></div>
            </li>
          {% endfor %}
        </ul>
      {% endfor %}
    </section>
  </noscript>
{% else %}
  <p class="topics-empty">No topics yet.</p>
{% endif %}

<script>
  document.addEventListener("DOMContentLoaded", function () {
    var controls = document.getElementById("topic-controls");
    if (!controls) return;

    var buttons = controls.querySelectorAll("[data-topic]");
    var posts = document.querySelectorAll("#topic-posts > li");
    var status = document.getElementById("topics-status");

    function buttonFor(topic) {
      for (var i = 0; i < buttons.length; i++) {
        if (buttons[i].dataset.topic === topic) return buttons[i];
      }
      return null;
    }

    // Filter the list to `topic` (empty string = show every post).
    function showTopic(topic) {
      var button = buttonFor(topic) || buttonFor("");
      topic = button.dataset.topic;

      var matches = 0;
      for (var i = 0; i < buttons.length; i++) {
        buttons[i].setAttribute("aria-pressed", buttons[i] === button ? "true" : "false");
      }
      for (var j = 0; j < posts.length; j++) {
        var shown = !topic || posts[j].dataset.topics.indexOf("|" + topic + "|") !== -1;
        posts[j].hidden = !shown;
        if (shown) matches++;
      }
      var noun = matches === 1 ? " post" : " posts";
      status.textContent = topic
        ? matches + noun + " in " + button.textContent + "."
        : "Showing all " + matches + noun + ".";
    }

    function applyHash() {
      var topic = window.location.hash ? decodeURIComponent(window.location.hash.slice(1)) : "";
      showTopic(topic);
    }

    for (var i = 0; i < buttons.length; i++) {
      buttons[i].addEventListener("click", function () {
        var topic = this.dataset.topic;
        if (topic) {
          window.location.hash = topic;
        } else {
          history.replaceState(null, "", window.location.pathname + window.location.search);
        }
        showTopic(topic);
      });
    }

    window.addEventListener("hashchange", applyHash);
    applyHash();
  });
</script>
