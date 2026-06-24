---
layout: page
icon: fas fa-link
order: 3
---

<div class="friend-list">
  {% for friend in site.data.friends %}
    <div class="friend-card">
      <img src="{{ friend.avatar }}" alt="{{ friend.name }}" class="friend-avatar" loading="lazy" nolightbox>
      <div class="friend-content">
        <div class="friend-name">{{ friend.name }}</div>
        <div class="friend-desc">{{ friend.description }}</div>
      </div>
      <a href="{{ friend.url }}" target="_blank" class="card-link" rel="noopener noreferrer"></a>
    </div>
  {% endfor %}
</div>

<style>
/* 移除页面标题下方的分隔线 */
.page-header {
  border-bottom: 0 !important;
  padding-bottom: 0 !important;
}

/* 列表容器（纵向排列） */
.friend-list {
  display: flex;
  flex-direction: column;
  gap: 0;
  margin-top: 24px;
}

/* 每个卡片：纵向布局，头像在上，文字在下 */
.friend-card {
  position: relative;          /* 为隐形链接提供定位上下文 */
  display: flex;
  flex-direction: column;      /* ⬅️ 纵向排列 */
  align-items: center;         /* 水平居中（可选，也可左对齐） */
  padding: 24px 20px 20px;
  border-radius: 16px;
  background: transparent;
  transition: background 0.25s ease;
  border-bottom: 1px solid rgba(128, 128, 128, 0.12);
  text-align: center;          /* 文字居中，也可以改为 left */
}

.friend-card:last-child {
  border-bottom: none;
}

.friend-card:hover {
  background: rgba(128, 128, 128, 0.05);
}

/* 头像 */
.friend-avatar {
  width: 72px;                /* 大一点更醒目 */
  height: 72px;
  border-radius: 50%;
  object-fit: cover;
  flex-shrink: 0;
  position: relative;
  z-index: 1;
  border: 2px solid var(--border-color, #e0e0e0);
  margin-bottom: 12px;        /* 与文字间距 */
}

/* 文字内容区 */
.friend-content {
  position: relative;
  z-index: 1;
  width: 100%;                /* 让内部文字撑满，方便控制对齐 */
}

.friend-name {
  font-size: 1.2rem;
  font-weight: 600;
  margin: 0;
  color: var(--text-color, #1f1f1f);
}

.friend-desc {
  font-size: 0.9rem;
  margin: 6px 0 0 0;
  color: var(--text-muted, #6c6c6c);
  /* 描述文字如果太长，可以折行或截断，这里让它自动换行 */
  word-break: break-word;
}

/* 整卡点击的隐形链接 */
.card-link {
  position: absolute;
  inset: 0;
  z-index: 0;
  text-decoration: none !important;
  border-radius: 16px;
}

/* ========== 深色模式适配 ========== */
[data-theme="dark"] .friend-card {
  border-bottom-color: rgba(255, 255, 255, 0.08);
}

[data-theme="dark"] .friend-card:hover {
  background: rgba(255, 255, 255, 0.04);
}

[data-theme="dark"] .friend-avatar {
  border-color: rgba(255, 255, 255, 0.15);
}

/* ========== 响应式 ========== */
@media (max-width: 576px) {
  .friend-card {
    padding: 18px 14px 16px;
  }
  .friend-avatar {
    width: 56px;
    height: 56px;
  }
  .friend-name {
    font-size: 1rem;
  }
  .friend-desc {
    font-size: 0.8rem;
  }
}
</style>