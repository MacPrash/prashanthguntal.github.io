---
layout: archive
permalink: /programme/
title: "Programs"
author_profile: true
header:
  image: "/images/Automation.jpg"
---

{% include base_path %}
{% include group-by-array collection=site.posts field="tags" %}

{% for tag in group_names %}
  {% assign posts = group_items[forloop.index0] %}
  {% if tag == "Automation" %}
    <h1 id="{{ tag | slugify }}" class="archive__subtitle">{{ tag }}</h1>
    {% for post in posts %}
      {% include archive-single.html %}
    {% endfor %}
  {% endif %}
{% endfor %}
