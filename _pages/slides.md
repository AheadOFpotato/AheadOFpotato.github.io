---
layout: page
title: slides
permalink: /slides/
description: Ideas, papers, and questions from group meetings.
nav: true
nav_order: 3
_styles: >
  .slides-intro { display: flex; align-items: baseline; justify-content: space-between; gap: 1rem; margin: 0 0 1.5rem; padding-bottom: 1rem; border-bottom: 1px solid var(--global-divider-color); }
  .slides-intro p { color: var(--global-text-color-light); margin: 0; }
  .slides-count { font-size: .75rem; letter-spacing: .08em; text-transform: uppercase; white-space: nowrap; }
  .slides-grid { display: grid; grid-template-columns: repeat(2, minmax(0, 1fr)); gap: 1.5rem; }
  .slide-card { display: flex; flex-direction: column; overflow: hidden; border: 1px solid var(--global-divider-color); border-radius: .65rem; background: var(--global-card-bg-color); transition: transform .2s, box-shadow .2s; }
  .slide-card:hover { transform: translateY(-3px); box-shadow: 0 10px 28px rgba(0, 0, 0, .1); }
  .slide-cover { display: block; aspect-ratio: 16 / 9; overflow: hidden; background: #fff; border-bottom: 1px solid var(--global-divider-color); }
  .slide-cover img { display: block; width: 100%; height: 100%; object-fit: contain; }
  .slide-body { display: flex; flex: 1; flex-direction: column; padding: 1.15rem 1.25rem 1.2rem; }
  .slide-meta { display: flex; justify-content: space-between; gap: .75rem; color: var(--global-text-color-light); font-size: .75rem; letter-spacing: .03em; }
  .slide-topic { color: var(--global-theme-color); font-weight: 600; }
  .slide-title { font-size: 1.15rem; line-height: 1.3; margin: .65rem 0 .5rem; }
  .slide-description { color: var(--global-text-color-light); font-size: .9rem; line-height: 1.55; margin: 0 0 1.1rem; }
  .slide-link { margin-top: auto; font-size: .84rem; font-weight: 600; }
  @media (max-width: 600px) { .slides-grid { grid-template-columns: 1fr; } .slides-intro { align-items: flex-start; } }
---

<div class="slides-intro">
  <p>Research talks &amp; reading notes, collected over time.</p>
  <span class="slides-count">{{ site.data.meeting_slides.slides.size }} presentations</span>
</div>

<div class="slides-grid">
  {% for slide in site.data.meeting_slides.slides %}
  <article class="slide-card">
    <a class="slide-cover" href="{{ slide.file | relative_url }}" aria-label="Open {{ slide.title | escape }} (PDF)">
      <img src="{{ slide.thumbnail | relative_url }}" alt="" loading="lazy">
    </a>
    <div class="slide-body">
      <div class="slide-meta"><span class="slide-topic">{{ slide.topic }}</span><time datetime="{{ slide.date | date: '%Y-%m-%d' }}">{{ slide.date | date: "%b %d, %Y" }}</time></div>
      <h2 class="slide-title">{{ slide.title }}</h2>
      <p class="slide-description">{{ slide.description }}</p>
      <a class="slide-link" href="{{ slide.file | relative_url }}">View slides <span aria-hidden="true">↗</span></a>
    </div>
  </article>
  {% endfor %}
</div>
