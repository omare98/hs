---
layout: page
title: Home
id: home
permalink: /
---

# 어서오세요! 🌱

<p style="padding: 3em 1em; background: #f5f7ff; border-radius: 4px;">
  해성그룹 HR 프로젝트 관련 공통문서들을 모아놓은 공간입니다.
</p>

<strong>최근 업데이트 된 문서 목록</strong>

<ul>
  {% assign recent_notes = site.notes | sort: "last_modified_at_timestamp" | reverse %}
  {% for note in recent_notes limit: 10 %}
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
