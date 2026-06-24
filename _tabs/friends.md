
---
layout: page          # 使用页面布局
title: 友链        # 标题
icon: fas fa-link    # 侧边栏显示的图标
order: 3             # 侧边栏显示顺序（数字越小越靠前）
---

<div class="friend-grid">
  {% for friend in site.data.friends %}
    <a href="{{ friend.url }}" target="_blank" class="friend-card">
      <img src="{{ friend.avatar }}" alt="{{ friend.name }}" class="friend-avatar">
      <div class="friend-info">
        <h3>{{ friend.name }}</h3>
        <p>{{ friend.description }}</p>
      </div>
    </a>
  {% endfor %}
</div>


