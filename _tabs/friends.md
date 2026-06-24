---
layout: page
title: 友链
icon: fas fa-link
order: 3
---

<div class="friend-list">
  {% for friend in site.data.friends %}
    <a href="{{ friend.url }}" target="_blank" class="friend-item">
      <img src="{{ friend.avatar }}" alt="{{ friend.name }}" class="friend-avatar">
      <div class="friend-info">
        <h3>{{ friend.name }}</h3>
        <p>{{ friend.description }}</p>
      </div>
    </a>
  {% endfor %}
</div>

<style>
/* 单列列表容器 */
.friend-list {
  display: flex;
  flex-direction: column;
  gap: 12px;
  margin-top: 20px;
}

/* 单个友链项：横向布局 */
.friend-item {
  display: flex;
  align-items: center;
  padding: 14px 16px;
  border-radius: 12px;
  background-color: var(--bg-secondary, #ffffff);
  text-decoration: none;
  color: inherit;
  transition: background-color 0.2s ease;
}

.friend-item:hover {
  background-color: var(--surface-hover, #f5f5f5);
}

/* 左侧小头像（圆角方形） */
.friend-avatar {
  width: 56px;
  height: 56px;
  border-radius: 12px;
  object-fit: cover;
  flex-shrink: 0;
}

/* 右侧文字区 */
.friend-info {
  margin-left: 16px;
  display: flex;
  flex-direction: column;
  gap: 4px;
  overflow: hidden;
}

.friend-info h3 {
  margin: 0;
  font-size: 18px;
  font-weight: 600;
  color: var(--text-primary, #1f1f1f);
}

.friend-info p {
  margin: 0;
  font-size: 14px;
  color: var(--text-secondary, #666666);
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

/* 深色模式自动适配 */
[data-theme="dark"] .friend-item {
  background-color: var(--bg-secondary);
}
[data-theme="dark"] .friend-item:hover {
  background-color: var(--surface-hover);
}
</style>
