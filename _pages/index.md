---
layout: page
title: Home
id: home
permalink: /
---

# Welcome! 🌱

<p style="padding: 3em 1em; background: #f5f7ff; border-radius: 4px;">
  Take a look at <span style="font-weight: bold">[[Your first note]]</span> to get started on your exploration.
</p>

This digital garden template is free, open-source, and [available on GitHub here](https://github.com/maximevaillancourt/digital-garden-jekyll-template).

The easiest way to get started is to read this [step-by-step guide explaining how to set this up from scratch](https://maximevaillancourt.com/blog/setting-up-your-own-digital-garden-with-jekyll).

<strong>Recently updated notes</strong>

<ul>
  {% assign recent_notes = site.notes | sort: "last_modified_at_timestamp" | reverse %}
  {% for note in recent_notes limit: 5 %}
    <li>
      {{ note.last_modified_at | date: "%Y-%m-%d" }} — <a class="internal-link" href="{{ site.baseurl }}{{ note.url }}">{{ note.title }}</a>
    </li>
  {% endfor %}
</ul>

<style>
  .wrapper {
    max-width: 46em;
  }
</style>

# Haesung Group e-HR 개발 가이드

## 1. [네이밍 가이드](/naiming-guide.md)

## 2. [자바 코딩 스타일 가이드](/Java-styleguide.md)

## 3. [자바스크립트 코딩 스타일 가이드](/Javascript-styleguide.md)

## 4. [SQL 스타일 가이드](/SQL-stlyeguide.md)

## 5. [공통 함수 및 컴포넌트](/common-guide.md)

## 6. [알림 서비스](/notif-guide.md)
