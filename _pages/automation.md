---
layout: archive
permalink: /automation/
title: "Automation"
author_profile: true
header:
  image: "/images/Automation.jpg"
---

{% include base_path %}
{% include group-by-array collection=site.posts field="tags" %}

{% for tag in group_names %}
  {% assign posts = group_items[forloop.index0] %}
   <h2 id="{{ tag | slugify }}" class="archive__subtitle">{{ tag }}</h2>
  {% if tag == "Automation" %}
    {% for post in posts %}
      {% include archive-single.html %}
    {% endfor %}
  {% endif %}
{% endfor %}
