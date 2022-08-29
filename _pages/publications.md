---
layout: page
permalink: /publications/
title: publications
description: preprints and publications in reversed chronological order.
years: [2022, 2021, 2019]
nav: true
nav_order: 1
---
<!-- _pages/publications.md -->
<div class="publications">

<h2 class="year">preprints</h2>
{% bibliography -f papers -q @*[preprint=true]* %}

{% for y in page.years %}
  <h2 class="year">{{y}}</h2>
  {% bibliography -f papers -q @*[year={{y}} && preprint!=true]* %}
{% endfor %}

</div>
