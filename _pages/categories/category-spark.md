---
title: "Spark"
layout: archive
permalink: /categories/spark
author_profile: true
sidebar_main: true
header:
  overlay_image : /assets/images/category/data.jpg
---

{% assign posts = site.categories.spark | sort:"date" %}

{% for post in posts %}
  {% include archive-single.html type=page.entries_layout %}
{% endfor %}
