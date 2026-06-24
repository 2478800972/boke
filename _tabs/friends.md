---
layout: page
icon: fas fa-link
order: 3
---

<div class="friend-list">
  {% for friend in site.data.friends %}
    <div class="friend-card">
      <img src="{{ friend.avatar }}" alt="{{ friend.name }}" class="friend-avatar" loading="lazy">
      <div class="friend-content">
        <div class="friend-name">{{ friend.name }}</div>
        <div class="friend-desc">{{ friend.description }}</div>
      </div>
      <a href="{{ friend.url }}" target="_blank" class="card-link" aria-label="访问{{ friend.name }}的博客"></a>
    </div>
  {% endfor %}
</div>

<style>
/* 移除页面标题底部分隔线 */
.page-header {
  border-bottom: 0 !important;
  padding-bottom: 0 !important;
}

/* 友链列表容器 */
.friend-list {
  display: flex;
  flex-direction: column;
  gap: 16px;
  margin-top: 20px;
}

/* 单个友链卡片 */
.friend-card {
  position: relative;
  display: flex;
  align-items: center;
  padding: 16px;
  border-radius: 16px;
  background-color: var(--bg-secondary, #ffffff);
  transition: background-color 0.2s ease;
}

.friend-card:hover {
  background-color: var(--surface-hover, #f5f5f5);
}

/* 左侧圆角头像 */
.friend-avatar {
  width: 64px;
  height: 64px;
  border-radius: 12px;
  object-fit: cover;
  flex-shrink: 0;
  z-index: 1;
}

/* 右侧文字区 */
.friend-content {
  margin-left: 16px;
  display: flex;
  flex-direction: column;
  gap: 6px;
  z-index: 1;
  overflow: hidden;
}

.friend-name {
  font-size: 20px;
  font-weight: 600;
  color: var(--text-primary, #1f1f1f);
  line-height: 1.2;
}

.friend-desc {
  font-size: 14px;
  color: var(--text-secondary, #666666);
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
  line-height: 1.2;
}

/* 整卡隐形点击层：点击卡片任意位置都能跳转 */
.card-link {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 0;
}

/* 深色模式自动适配 */
[data-theme="dark"] .friend-card {
  background-color: var(--bg-secondary);
}
[data-theme="dark"] .friend-card:hover {
  background-color: var(--surface-hover);
}
</style>
