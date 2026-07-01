---
layout: page
title: 推文封面
permalink: /gallery/fm/
---

<!-- 1. 返回按钮 -->
<div class="gallery-album-back">
  <button onclick="window.location.href='{{ site.baseurl }}/gallery/'" 
          class="back-to-gallery-btn">
    ← 返回相册列表
  </button>
</div>

<!-- 2. 提取数据逻辑 -->
{% assign album = site.data.gallery | where: "id", "fm" | first %}

<!-- 3. 相册网格容器 -->
<div class="album-grid-wrapper">
  {% if album %}
    <div class="album-grid-header">
      <h3 class="album-grid-month">2026 年 6 月</h3>
    </div>
    
    <div class="album-grid-gallery">
      {% for photo in album.photos %}
        <div class="album-grid-item">
          <img src="{{ photo | absolute_url }}" alt="相册照片" loading="lazy" onerror="this.style.display='none'">
        </div>
      {% endfor %}
    </div>
  {% else %}
    <p style="color: var(--text-color, #555); margin-top: 20px;">⚠️ 未找到相册数据，请检查 _data/gallery.yml 是否配置正确。</p>
  {% endif %}
</div>

<!-- 4. 自适应比例 + 圆角修复样式 -->
<style>
/* 全局盒模型统一 */
.album-grid-wrapper *, 
.album-grid-wrapper *::before, 
.album-grid-wrapper *::after {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

.gallery-album-back { margin-bottom: 20px; }
.back-to-gallery-btn {
    border: none; 
    background: transparent; 
    font-size: 1rem; 
    font-weight: 500; 
    padding: 0;
    cursor: pointer; 
    text-decoration: none; 
    color: var(--text-color, #333); 
    transition: color 0.2s ease;
}
.back-to-gallery-btn:hover { color: var(--link-color, #DF9193); }

.album-grid-header { margin: 25px 0 15px; }
.album-grid-month { 
    font-size: 1.1rem; 
    font-weight: 500; 
    color: var(--text-color, #333); 
    margin: 0; 
    border-left: 3px solid var(--text-color, #333); 
    padding-left: 10px; 
}

/* 核心网格：宽度自适应列数，高度由图片自然撑开 */
.album-grid-gallery { 
    display: grid; 
    grid-template-columns: repeat(auto-fill, minmax(160px, 1fr));
    gap: 12px; 
    width: 100%;
    align-items: start; /* 顶部对齐，避免不同比例图片出现留白 */
}

/* 图片容器：去掉固定比例，边框完全跟随图片尺寸自适应 */
.album-grid-item { 
    position: relative; 
    width: 100%;
    border-radius: 10px; 
    overflow: hidden; 
    background: transparent;
    /* 保留圆角修复属性 */
    contain: paint;
    -webkit-mask-image: linear-gradient(#000, #000);
    mask-image: linear-gradient(#000, #000);
    -webkit-mask-size: 100% 100%;
    mask-size: 100% 100%;
    line-height: 0; /* 消除图片底部行高间隙 */
}

/* 图片：宽度撑满容器，高度按原始比例自适应，边框完美贴合 */
.album-grid-item img { 
    display: block; 
    width: 100%; 
    height: auto; /* 高度随图片比例自动计算 */
    object-fit: contain; /* 完整展示图片，不裁剪 */
    transition: transform 0.4s ease; 
    cursor: pointer; 
    border: none;
    /* 图片自身圆角兜底 */
    border-radius: 10px;
}

.album-grid-item:hover img { transform: scale(1.06); }

/* 移动端适配 */
@media (max-width: 480px) {
  .album-grid-gallery { 
    grid-template-columns: repeat(2, 1fr); 
    gap: 8px; 
  }
}

@media (min-width: 481px) and (max-width: 768px) {
  .album-grid-gallery { 
    grid-template-columns: repeat(3, 1fr); 
  }
}
</style>
