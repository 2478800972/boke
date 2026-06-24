---
layout: page          # 使用页面布局
title: 友链        # 标题
icon: fas fa-link    # 侧边栏显示的图标
order: 3             # 侧边栏显示顺序（数字越小越靠前）
---

{% for group in site.data.friends %}
  <h2 class="friends-group-title">{{ group.group_name }}</h2>
  <ul class="friends-list">
    {% for item in group.friends %}
      <li>
        <a href="{{ item.url }}" class="friend-item" target="_blank" rel="noopener noreferrer">
          <img src="{{ item.avatar | relative_url }}" alt="{{ item.name }}" class="friend-avatar" loading="lazy">
          <div class="friend-info">
            <h3 class="friend-name">{{ item.name }}</h3>
            <p class="friend-desc">{{ item.desc }}</p>
          </div>
        </a>
      </li>
    {% endfor %}
  </ul>
{% endfor %}

<style>
.friends-group-title {
  margin: 2rem 0 0.75rem 0;
  font-size: 1.375rem;
  font-weight: 700;
  color: var(--text-primary);
}
.friends-group-title:first-of-type {
  margin-top: 1rem;
}

.friends-list {
  list-style: none;
  padding: 0;
  margin: 0;
  display: flex;
  flex-direction: column;
  gap: 0.75rem;
}

.friend-item {
  display: flex;
  align-items: center;
  padding: 0.875rem 1rem;
  border-radius: 12px;
  background-color: var(--card-bg);
  text-decoration: none;
  color: inherit;
  transition: background-color 0.2s ease, transform 0.2s ease;
}
.friend-item:hover {
  background-color: var(--hover-bg);
  transform: translateY(-1px);
}

.friend-avatar {
  width: 56px;
  height: 56px;
  border-radius: 12px;
  object-fit: cover;
  flex-shrink: 0;
}

.friend-info {
  margin-left: 16px;
  display: flex;
  flex-direction: column;
  gap: 4px;
  overflow: hidden;
}

.friend-name {
  margin: 0;
  font-size: 18px;
  font-weight: 600;
  color: var(--text-primary);
}

.friend-desc {
  margin: 0;
  font-size: 14px;
  color: var(--text-secondary);
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

@media (max-width: 576px) {
  .friend-avatar {
    width: 44px;
    height: 44px;
  }
  .friend-item {
    padding: 12px 14px;
  }
}
</style>
