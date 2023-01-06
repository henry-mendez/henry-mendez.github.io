---
title:  "Blog"
layout: archive
permalink: /blog/
author_profile: true
comments: true
---
My blogs are written mainly for my own reference as I am learning new things. Should someone else find these useful too, I am happy for it!
{%for post in site.posts %}
    {% include archive-single.html %}
{% endfor %}
