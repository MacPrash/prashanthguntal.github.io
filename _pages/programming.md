---
layout: archive
permalink: /programme/
title: "Programs"
author_profile: true
header:
  image: "/images/Automation.jpg"
---

{% include base_path %}
{% include group-by-array collection=site.posts field="tag" %}


{% for tag in group_names %}
  {% if tag == "Programming"}
  {% assign posts = group_items[forloop.index0] %}
  <h2 id="{{ tag | slugify }}" class="archive__subtitle">{{ tag }}</h2>
  {% for post in posts %}
    {% include archive-single.html %}
  {% endfor %}
  {% endif %}
{% endfor %}