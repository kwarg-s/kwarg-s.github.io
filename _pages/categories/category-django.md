---
title: "Spark"
layout: archive
permalink: /categories/django
author_profile: true
sidebar_main: true
header:
  overlay_image : /assets/images/main.jpg
---

{% assign posts = site.categories.django | sort:"date" %}

{% for post in posts %}
  {% include archive-single.html type=page.entries_layout %}
{% endfor %}
