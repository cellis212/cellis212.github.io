---
layout: page
permalink: /research/
title: research
description: Peer-reviewed publications and working papers. Click a topic to filter.
nav: true
nav_order: 1
---

<style>
  /* Do not underline my own name in author lists */
  .publications ol.bibliography li .author > em {
    border-bottom: none;
  }

  /* Topic cloud */
  .topic-cloud-label {
    text-align: center;
    color: var(--global-text-color-light);
    font-size: 0.85rem;
    letter-spacing: 0.03em;
    text-transform: uppercase;
    margin-bottom: 0.6rem;
  }
  .topic-cloud {
    display: flex;
    flex-wrap: wrap;
    gap: 0.4rem 0.55rem;
    align-items: baseline;
    justify-content: center;
    margin: 0 auto 2.5rem;
    max-width: 60rem;
    line-height: 1.6;
  }
  .topic-tag {
    border: 1px solid var(--global-divider-color);
    background: transparent;
    color: var(--global-text-color);
    border-radius: 999px;
    padding: 0.18em 0.7em;
    cursor: pointer;
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
  .topic-tag.tag-all { font-weight: 600; }
  .topic-count { opacity: 0.55; font-size: 0.7em; margin-left: 0.2em; }
</style>

<div class="topic-cloud-label">Filter by topic</div>
<div class="topic-cloud" id="topic-cloud"></div>

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
    var sections = Array.prototype.slice.call(document.querySelectorAll('.topic-section'));

    // Build [ {li, tags[]} ] from each paper's data-tags, and a tag -> count map.
    var papers = [];
    var counts = {};
    document.querySelectorAll('.topic-section [data-tags]').forEach(function (row) {
      var li = row.closest('li');
      if (!li) return;
      var tags = (row.getAttribute('data-tags') || '')
        .split(',').map(function (t) { return t.trim(); }).filter(Boolean);
      papers.push({ li: li, tags: tags });
      tags.forEach(function (t) { counts[t] = (counts[t] || 0) + 1; });
    });

    var unique = Object.keys(counts).sort(function (a, b) {
      return a.toLowerCase().localeCompare(b.toLowerCase());
    });
    var vals = unique.map(function (t) { return counts[t]; });
    var min = Math.min.apply(null, vals);
    var max = Math.max.apply(null, vals);

    var active = null; // null = show all

    function render() {
      sections.forEach(function (s) {
        var visible = 0;
        papers.forEach(function (p) {
          if (p.li.closest('.topic-section') !== s) return;
          var show = (active === null) || p.tags.indexOf(active) !== -1;
          p.li.style.display = show ? '' : 'none';
          if (show) visible++;
        });
        s.style.display = visible === 0 ? 'none' : '';
      });
      cloud.querySelectorAll('.topic-tag').forEach(function (t) {
        var f = t.getAttribute('data-filter');
        t.classList.toggle('active', (f === '__all__' && active === null) || f === active);
      });
    }

    function makeTag(label, filter, count) {
      var b = document.createElement('button');
      b.type = 'button';
      b.className = 'topic-tag' + (filter === '__all__' ? ' tag-all' : '');
      b.setAttribute('data-filter', filter);
      b.textContent = label;
      if (count != null) {
        if (max !== min) {
          b.style.fontSize = (0.8 + (count - min) / (max - min) * 0.9).toFixed(2) + 'rem';
        }
        var span = document.createElement('span');
        span.className = 'topic-count';
        span.textContent = count;
        b.appendChild(document.createTextNode(' '));
        b.appendChild(span);
      }
      b.addEventListener('click', function () {
        active = (filter === '__all__' || filter === active) ? null : filter;
        render();
      });
      return b;
    }

    cloud.appendChild(makeTag('All', '__all__', null));
    unique.forEach(function (t) { cloud.appendChild(makeTag(t, t, counts[t])); });
    render();
  });
</script>
