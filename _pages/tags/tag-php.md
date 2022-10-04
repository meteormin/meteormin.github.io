---
title: "PHP"
layout: archive
permalink: tags/php
author_profile: true
sidebar_main: true
---

{% assign posts = site.tags.php %}
{% for post in posts %} {% include archive-single.html type=page.entries_layout %} {% endfor %}
