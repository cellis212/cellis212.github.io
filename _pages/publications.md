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

## Household Finance and Disaster Risk

**Refereed Publications**

<div class="publications">
{% bibliography -f papers -q @article[topic=household] -g none %}
</div>

## Health Economics and Healthcare Finance

**Refereed Publications**

<div class="publications">
{% bibliography -f papers -q @article[topic=health] -g none %}
</div>

**Working Papers**

<div class="publications">
{% bibliography -f papers -q @unpublished[topic=health] -g none %}
</div>

## Life Insurance and Annuities

**Refereed Publications**

<div class="publications">
{% bibliography -f papers -q @article[topic=life] -g none %}
</div>

**Working Papers**

<div class="publications">
{% bibliography -f papers -q @unpublished[topic=life] -g none %}
</div>

## Miscellaneous

**Refereed Publications**

<div class="publications">
{% bibliography -f papers -q @article[topic=misc] -g none %}
</div>

**Working Papers**

<div class="publications">
{% bibliography -f papers -q @unpublished[topic=misc] -g none %}
</div>
