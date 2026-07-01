---
layout: page
permalink: /gallery/
icon: fas fa-images
order: 4
---

<!-- 用 Liquid 循环生成相册卡片 -->
<div class="album-grid">
  <div class="album-grid__list">
    {% for album in site.data.gallery %}
    
    <button class="album-grid__item" 
            onclick="window.location.href='{{ site.baseurl }}/gallery/{{ album.id }}/'">
      
      <!-- 容器内层，用来承载绝对定位的图片 -->
      <div class="album-grid__card">
        <img class="album-grid__cover" src="{{ album.cover }}" alt="{{ album.title }}" loading="lazy">
        <div class="album-grid__shade"></div>
        <div class="album-grid__content">
          <h2 class="album-grid__title">{{ album.title }}</h2>
          <p class="album-grid__desc">{{ album.desc }}</p>
          
          <!-- ✨ 改动处：加上了 if 判断，没地点时隐藏整行和📍符号 -->
          {% if album.location %}
          <div class="album-grid__meta"><span class="album-grid__location">📍 {{ album.location }}</span></div>
          {% endif %}
          
          <div class="album-grid__tags">
            {% for tag in album.tags %}
            <span class="album-grid__tag">{{ tag }}</span>
            {% endfor %}
          </div>
        </div>
      </div>
      
    </button>
    
    {% endfor %}
  </div>
</div>

<style>
/* 核心自适应布局 CSS */
.album-grid__list { display: grid; grid-template-columns: repeat(auto-fill, minmax(280px, 1fr)); gap: 20px; margin-top: 20px; }

/* button 本身就是卡片容器 */
.album-grid__item { 
    display: block; 
    width: 100%; 
    aspect-ratio: 16 / 9; /* 强制锁定相册比例 */
    position: relative; 
    border: none; 
    padding: 0; 
    cursor: pointer; 
    border-radius: 12px; 
    overflow: hidden; 
    box-shadow: 0 4px 12px rgba(0,0,0,0.1); 
    transition: transform 0.3s; 
    background-color: #1a1a1a;
}
.album-grid__item:hover { transform: translateY(-5px); }

.album-grid__card { 
    display: block; 
    position: relative; 
    width: 100%; 
    height: 100%;
    pointer-events: none; /* 阻止内层冒泡，保证点击精准跳到最外层 button */
}

/* 核心修复：图片 100% 绝对定位，完全不依赖外部高度，彻底填满无死角 */
.album-grid__cover { 
    position: absolute; 
    top: 0; left: 0; 
    width: 100%; height: 100%; 
    object-fit: cover; 
    display: block; 
}

.album-grid__shade { position: absolute; inset: 0; background: linear-gradient(to bottom, rgba(0,0,0,0.1), rgba(0,0,0,0.7)); z-index: 1; }
.album-grid__content { position: absolute; bottom: 0; left: 0; right: 0; padding: 15px; z-index: 2; color: #fff; text-align: left;}

/* 大标题（纯白 + 阴影） */
.album-grid__title { font-size: 1.2rem; margin: 0; font-weight: 600; color: #ffffff; text-shadow: 0 1px 4px rgba(0, 0, 0, 0.6); }

.album-grid__desc { font-size: 0.9rem; opacity: 0.9; margin: 5px 0; display: -webkit-box; -webkit-line-clamp: 2; -webkit-box-orient: vertical; overflow: hidden; }
.album-grid__count { position: absolute; top: 10px; right: 10px; background: rgba(0,0,0,0.5); padding: 4px 10px; border-radius: 20px; font-size: 0.8rem; z-index: 2; color: #fff; }
.album-grid__location { font-size: 0.8rem; opacity: 0.9; }
.album-grid__tags { margin-top: 5px; display: flex; flex-wrap: wrap; gap: 5px; }
.album-grid__tag { background: rgba(255,255,255,0.2); padding: 2px 8px; border-radius: 12px; font-size: 0.7rem; }
</style>