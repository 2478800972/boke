---
layout: page
icon: fas fa-link
order: 3
---

<div class="friend-list">
  {% for friend in site.data.friends %}
    <div class="friend-item">
      <img src="{{ friend.avatar }}" alt="{{ friend.name }}" class="friend-avatar" loading="lazy">
      <div class="friend-info">
        <a href="{{ friend.url }}" target="_blank" class="friend-name">{{ friend.name }}</a>
        <p>{{ friend.description }}</p>
      </div>
      <!-- 整卡点击跳转的隐形链接 -->
      <a href="{{ friend.url }}" target="_blank" class="friend-link-mask" aria-label="访问{{ friend.name }}的博客"></a>
    </div>
  {% endfor %}
</div>

<style>
/* 移除页面标题下方的分隔线 */
.page-header {
  border-bottom: 0 !important;
  padding-bottom: 0 !important;
}

/* 单列列表容器 */
.friend-list {
  display: flex;
  flex-direction: column;
  gap: 12px;
  margin-top: 20px;
}

/* 单个友链卡片 */
.friend-item {
  position: relative;
  display: flex;
  align-items: center;
  padding: 14px 16px;
  border-radius: 12px;
  background-color: var(--bg-secondary, #ffffff);
  transition: background-color 0.2s ease;
}

.friend-item:hover {
  background-color: var(--surface-hover, #f5f5f5);
}

/* 左侧头像 */
.friend-avatar {
  width: 56px;
  height: 56px;
  border-radius: 12px;
  object-fit: cover;
  flex-shrink: 0;
  z-index: 1;
}

/* 右侧文字区 */
.friend-info {
  margin-left: 16px;
  display: flex;
  flex-direction: column;
  gap: 4px;
  overflow: hidden;
  z-index: 1;
}

.friend-name {
  font-size: 18px;
  font-weight: 600;
  color: var(--text-primary, #1f1f1f);
  text-decoration: none;
}

.friend-info p {
  margin: 0;
  font-size: 14px;
  color: var(--text-secondary, #666666);
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

/* 整卡隐形点击层：覆盖整张卡片，实现点击任意位置跳转 */
.friend-link-mask {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 0;
}

/* 深色模式适配 */
[data-theme="dark"] .friend-item {
  background-color: var(--bg-secondary);
}
[data-theme="dark"] .friend-item:hover {
  background-color: var(--surface-hover);
}
[data-theme="dark"] .friend-name {
  color: var(--text-primary);
}
</style>
