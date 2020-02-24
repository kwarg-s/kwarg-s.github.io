---
title: "Etc"
layout: archive
permalink: /categories/etc
author_profile: true
sidebar_main: true
header:
  overlay_image : /assets/images/main.jpg
---

{% assign posts = site.categories.etc | sort:"date" %}

{% for post in posts %}
  {% include archive-single.html type=page.entries_layout %}
{% endfor %}
