---
# the default layout is 'page'
icon: fas fa-info-circle
order: 4
permalink: /about/
---

![派蒙](/assets/img/da.png)

嗨旅行者，恭喜你发现了一个宝藏的博客，本站采用 Jekyll 主题 Chirpy，是个人博客(˵¯͒〰¯͒˵)

欢迎你的来访！

---


<!-- 美化友链区块 - 夜间沉浸版 -->
<div class="friend-links" style="display: flex; flex-wrap: wrap; gap: 24px; margin: 40px 0;">

  <!-- 主题与官网 -->
  <div class="link-card">
    <h4><span>📖</span> 阅读主题</h4>
    <ul>
      <li><a href="https://2478800972.github.io/momo/zhuti.html" target="_blank">✨ 兮心阅读主题</a></li>
      <li><a href="https://2478800972.github.io/momo/" target="_blank">🏠 兮心官网</a></li>
    </ul>
  </div>

  <!-- 教程与资源 -->
  <div class="link-card">
    <h4><span>📚</span> 教程与资源</h4>
    <ul>
      <li><a href="https://flowus.cn/moshang/share/3d76e1ce-55ca-4cd4-ab99-8f8b486c56ae" target="_blank">📘 阅读基础使用教程</a></li>
      <li><a href="https://2478800972.github.io/momo/发布/书源发布.html" target="_blank">🔖 书源发布页</a></li>
    </ul>
  </div>

  <!-- 下载 -->
  <div class="link-card">
    <h4><span>⬇️</span> 下载</h4>
    <ul>
      <li><a href="https://2478800972.github.io/momo/下载/xz.html" target="_blank">📱 阅读下载</a></li>
    </ul>
  </div>

</div>

<style>
  /* ----- 基础样式（日间 & 夜间通用） ----- */
  .link-card {
    flex: 1;
    min-width: 220px;
    background: #f8f9fa;
    border-radius: 16px;
    padding: 20px 18px;
    box-shadow: 0 4px 12px rgba(0,0,0,0.05);
    transition: background 0.3s ease, box-shadow 0.3s ease, border-color 0.3s ease;
    border: 1px solid transparent;
  }

  .link-card h4 {
    margin-top: 0;
    margin-bottom: 16px;
    font-size: 1.3rem;
    display: flex;
    align-items: center;
    gap: 8px;
    border-bottom: 2px solid #e9ecef;
    padding-bottom: 10px;
    transition: border-color 0.3s ease;
    color: #2c3e50;
  }

  .link-card ul {
    list-style: none;
    padding-left: 0;
    margin: 0;
  }

  .link-card li {
    margin-bottom: 10px;
  }

  .link-card a {
    text-decoration: none;
    color: #2c3e50;
    font-weight: 500;
    border-bottom: 1px solid transparent;
    transition: color 0.2s, border-color 0.2s;
  }

  .link-card a:hover {
    border-bottom-color: #0366d6;
    color: #0366d6;
  }

  /* ----- 🌙 夜间沉浸模式（自动适配系统暗色） ----- */
  @media (prefers-color-scheme: dark) {
    .link-card {
      background: rgba(30, 30, 35, 0.85);        /* 半透明深色背景，融入暗色页面 */
      backdrop-filter: blur(2px);                 /* 轻微磨砂，增强沉浸 */
      box-shadow: 0 4px 20px rgba(0, 0, 0, 0.6);  /* 更深的阴影，层次感 */
      border-color: rgba(255, 255, 255, 0.06);    /* 极淡边框，不刺眼 */
    }

    .link-card h4 {
      color: #e8edf3;                             /* 柔白标题 */
      border-bottom-color: rgba(255, 255, 255, 0.1);
    }

    .link-card a {
      color: #b0c4de;                             /* 淡蓝灰文字，温和不累眼 */
    }

    .link-card a:hover {
      border-bottom-color: #6ea8fe;               /* 高亮色更亮但不过曝 */
      color: #8ab4f8;
    }

    /* 可选：让 emoji 在暗色下稍微降低饱和度，更协调 */
    .link-card h4 span,
    .link-card a span {
      filter: brightness(0.9);
    }
  }

  /* 如果页面本身有深色背景，还可以加上全局适配 */
  body {
    transition: background 0.3s, color 0.3s;
  }
</style>