---
layout: page
title: 友链
icon: fas fa-link
order: 3
---

{% for group in site.data.friends %}
  <h2 class="friends-group-title">{{ group.group_name }}</h2>
  <ul class="friends-list">
    {% for item in group.friends %}
      <li>
        <a href="{{ item.url }}" class="friend-item" target="_blank" rel="noopener noreferrer">
          <img src="{{ item.avatar }}" alt="{{ item.name }}" class="friend-avatar" loading="lazy">
          <div class="friend-info">
            <h3 class="friend-name">{{ item.name }}</h3>
            <p class="friend-desc">{{ item.desc }}</p>
          </div>
        </a>
      </li>
    {% endfor %}
  </ul>
{% endfor %}
