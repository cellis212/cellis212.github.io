---
layout: page
permalink: /research/
title: research
description: Peer-reviewed publications and working papers, grouped by topic.
nav: true
nav_order: 1
---

<style>
  /* Do not underline my own name in author lists */
  .publications ol.bibliography li .author > em {
    border-bottom: none;
  }

  /* Clickable topic cloud */
  .topic-cloud {
    display: flex;
    flex-wrap: wrap;
    gap: 0.5rem 0.75rem;
    align-items: baseline;
    justify-content: center;
    margin: 0.5rem 0 2.5rem;
  }
  .topic-tag {
    border: 1px solid var(--global-divider-color);
    background: transparent;
    color: var(--global-text-color);
    border-radius: 999px;
    padding: 0.28em 0.9em;
    cursor: pointer;
    line-height: 1.25;
    font-family: inherit;
    transition: color 0.15s ease, border-color 0.15s ease, background-color 0.15s ease;
  }
  .topic-tag:hover {
    border-color: var(--global-theme-color);
    color: var(--global-theme-color);
  }
  .topic-tag.active {
    background: var(--global-theme-color);
    border-color: var(--global-theme-color);
    color: #fff;
  }
  .topic-count { opacity: 0.6; font-size: 0.72em; margin-left: 0.25em; }
</style>

<div class="topic-cloud" id="topic-cloud">
  <button type="button" class="topic-tag active" data-filter="all">All</button>
  <button type="button" class="topic-tag" data-filter="household">Household Finance &amp; Disaster Risk</button>
  <button type="button" class="topic-tag" data-filter="health">Health Economics &amp; Healthcare Finance</button>
  <button type="button" class="topic-tag" data-filter="life">Life Insurance &amp; Annuities</button>
  <button type="button" class="topic-tag" data-filter="misc">Miscellaneous</button>
</div>

<div class="topic-section" data-topic="household">
  <h2>Household Finance and Disaster Risk</h2>
  <div class="publications">
  {% bibliography -f papers -q @*[topic=household] -g none %}
  </div>
</div>

<div class="topic-section" data-topic="health">
  <h2>Health Economics and Healthcare Finance</h2>
  <div class="publications">
  {% bibliography -f papers -q @*[topic=health] -g none %}
  </div>
</div>

<div class="topic-section" data-topic="life">
  <h2>Life Insurance and Annuities</h2>
  <div class="publications">
  {% bibliography -f papers -q @*[topic=life] -g none %}
  </div>
</div>

<div class="topic-section" data-topic="misc">
  <h2>Miscellaneous</h2>
  <div class="publications">
  {% bibliography -f papers -q @*[topic=misc] -g none %}
  </div>
</div>

<script>
  document.addEventListener('DOMContentLoaded', function () {
    var cloud = document.getElementById('topic-cloud');
    if (!cloud) return;
    var tags = Array.prototype.slice.call(cloud.querySelectorAll('.topic-tag'));
    var sections = Array.prototype.slice.call(document.querySelectorAll('.topic-section'));

    // Count papers per topic (self-updating as papers are added).
    var counts = {};
    sections.forEach(function (s) {
      counts[s.getAttribute('data-topic')] = s.querySelectorAll('.bibliography > li').length;
    });
    var vals = Object.keys(counts).map(function (k) { return counts[k]; });
    var min = Math.min.apply(null, vals);
    var max = Math.max.apply(null, vals);

    // Scale each topic tag's size by its paper count and append the count.
    tags.forEach(function (tag) {
      var f = tag.getAttribute('data-filter');
      if (f === 'all') return;
      var c = counts[f] || 0;
      var scale = (max === min) ? 1 : (c - min) / (max - min);
      tag.style.fontSize = (0.9 + scale * 0.9).toFixed(2) + 'rem';
      var span = document.createElement('span');
      span.className = 'topic-count';
      span.textContent = '(' + c + ')';
      tag.appendChild(document.createTextNode(' '));
      tag.appendChild(span);
    });

    function apply(filter) {
      tags.forEach(function (t) {
        t.classList.toggle('active', t.getAttribute('data-filter') === filter);
      });
      sections.forEach(function (s) {
        s.style.display = (filter === 'all' || s.getAttribute('data-topic') === filter) ? '' : 'none';
      });
    }

    tags.forEach(function (tag) {
      tag.addEventListener('click', function () { apply(tag.getAttribute('data-filter')); });
    });
  });
</script>
