---
layout: page
permalink: /research/
title: research
description: Peer-reviewed publications and working papers, grouped by topic.
nav: true
nav_order: 1
---

<!-- Bibsearch Feature -->
{% include bib_search.liquid %}

<style>
  /* Do not underline my own name in author lists */
  .publications ol.bibliography li .author > em {
    border-bottom: none;
  }
</style>

## Household Finance and Disaster Risk

<div class="publications">
{% bibliography -f papers -q @*[topic=household] -g none %}
</div>

## Health Economics and Healthcare Finance

<div class="publications">
{% bibliography -f papers -q @*[topic=health] -g none %}
</div>

## Life Insurance and Annuities

<div class="publications">
{% bibliography -f papers -q @*[topic=life] -g none %}
</div>

## Miscellaneous

<div class="publications">
{% bibliography -f papers -q @*[topic=misc] -g none %}
</div>
