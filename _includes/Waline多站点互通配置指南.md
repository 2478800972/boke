---
title: Waline 多站点互通配置指南
description: 两个博客站点共用同一个 Waline 评论系统，但评论数据不互通。
author: 墨殇
date: 2026-06-26 20:00:00 +0800
categories: [教程, 技术]
tags: [Chirpy, Waline, 功能]
pin:                 # true 表示置顶，false 或不写则不置顶
math: true                # 启用数学公式（需要时设为 true）
mermaid: true             # 启用流程图（需要时设为 true）
image:
  path: https://cdn.jsdmirror.com/gh/2478800972/boke@main/assets/img/tw/wz/绣球花1.png      # 预览图路径（注意加上 /boke 前缀）
  alt: 美丽的绣球花ᜊ•͈⌔•͈ᜊ
comments: true
---


# Waline 评论系统多站点数据互通完整配置指南

## 📌 问题背景

两个博客站点共用同一个 Waline 评论系统，但评论数据不互通：
- `https://2478800972.github.io/boke/posts/wz/`（路径带 `/boke`）
- `https://momoboke.cc.cd/posts/wz/`（路径不带 `/boke`）
- `https://boke-9u8.pages.dev/posts/wz/`（路径不带 `/boke`）

**根本原因**：Waline 通过 `path` 参数区分文章，三个站点提交的 `path` 不一致（`/boke/posts/wz/` vs `/posts/wz/`），导致评论数据被分散存储。

## 🔧 第一阶段：修复 Waline 后台登录问题

### 1.1 添加安全域名（`SECURE_DOMAINS`）

在 Vercel 项目环境变量中添加：

| 变量名 | 变量值 |
|--------|--------|
| `SECURE_DOMAINS` | `2478800972.github.io,momoboke.cc.cd,boke-9u8.pages.dev,waline.cc.cd` |

**注意**：
- 多个域名用英文逗号 `,` 分隔，不要有空格
- 不要带 `http://` 或 `https://`
- **必须包含 `waline.cc.cd`**，否则无法登录后台

**操作步骤**：
1. Vercel 项目 → Settings → Environment Variables
2. 添加 `SECURE_DOMAINS`，填入上述域名列表
3. 保存后，进入 Deployments → 点击最新部署右侧的 `···` → **Redeploy**（必须重新部署！）

### 1.2 重置管理员密码

如果仍无法登录后台，通过环境变量强制重置密码：

| 变量名 | 变量值 |
|--------|--------|
| `WALINE_ADMIN_EMAIL` | `你的邮箱@example.com` |
| `WALINE_ADMIN_PASSWORD` | `你的新密码` |

添加后**重新部署（Redeploy）**，然后用该邮箱和密码登录。

> ⚠️ **登录成功后建议删除这两个变量**，避免每次部署时覆盖数据库中的管理员信息。

## 🗄️ 第二阶段：数据库操作

### 2.1 确认表结构

Waline 使用 **Neon PostgreSQL** 数据库，相关表如下：

| 表名 | 用途 |
|------|------|
| `wl_comment` | 存储评论内容 |
| `wl_counter` | 存储评论计数 |
| `wl_users` | 存储用户信息 |

**关键字段**：`wl_comment` 表中存储文章路径的字段是 **`url`**（不是 `path`）。

### 2.2 查看当前所有路径

```sql
SELECT DISTINCT url FROM wl_comment;
```

### 2.3 迁移历史评论数据

将 /boke/posts/wz/ 路径下的评论合并到 /posts/wz/：

```sql
-- 单条迁移
UPDATE wl_comment SET url = '/posts/wz/' WHERE url = '/boke/posts/wz/';

-- 多条迁移（如有多个路径需要合并）
UPDATE wl_comment SET url = '/posts/wz/' WHERE url IN ('/boke/posts/wz/', '/book/posts/wz/');
```

### 2.4 用户管理（可选）

将普通用户升级为管理员：

```sql
UPDATE wl_users SET type = 'administrator' WHERE email = '用户邮箱@example.com';
```

删除用户：

```sql
DELETE FROM wl_users WHERE email = '用户邮箱@example.com';
```

> ⚠️ 执行前请确认至少保留一个管理员账号。

## 💻 第三阶段：前端代码修改

Waline 前端有两种加载方式：UMD（传统 Script 标签）和 ESM（ES Module）。根据你的主题实际使用的版本，选择对应方案修改。

### 3.1 方案一：UMD 版本（通用 Script 标签）

特征：使用 `<script src="...">` 加载，调用方式为 Waline.init({ ... })。

修改要点：在 Waline.init({}) 配置对象中，添加 path 选项，统一去掉路径中的 /boke。

完整代码示例（UMD 版）：

```html
{% if page.comments %}
<!-- Waline 评论区 - 黑色主题 + 明暗适配 + 移动端防遮挡 + 背景图置顶 + 隐藏版本号 -->
<section class="mt-5 pt-4 border-top">
  <h4 class="mb-4 fw-medium">{{ site.data.locales[lang].comment.comments }}</h4>
  <div id="waline"></div>
</section>

<link rel="stylesheet" href="https://unpkg.com/@waline/client@v3/dist/waline.css" />

<style>
  /* ========== 全局主题：黑色系 ========== */
  :root {
    --waline-theme-color: #000000;
    --waline-active-color: #333333;
    --waline-color: var(--text-color, #333);
    --waline-bgcolor: var(--body-bg, #fff);
    --waline-bgcolor-light: var(--card-bg, #f8f9fa);
    --waline-border-color: var(--border-color, #e9ecef);
    --waline-code-bgcolor: var(--code-bg, #f6f8fa);
    --waline-info-color: var(--text-muted, #6c757d);
    --waline-bq-color: var(--blockquote-bg, #f0f0f0);
    --waline-avatar-radius: 50%;
    --waline-font-size: 0.9375rem;
  }

  html[data-mode="dark"] {
    --waline-theme-color: #ffffff;
    --waline-active-color: #dddddd;
    --waline-color: var(--text-color, #c9d1d9);
    --waline-bgcolor: var(--body-bg, #0d1117);
    --waline-bgcolor-light: var(--card-bg, #161b22);
    --waline-border-color: var(--border-color, #30363d);
    --waline-code-bgcolor: var(--code-bg, #161b22);
    --waline-info-color: var(--text-muted, #8b949e);
    --waline-bq-color: var(--blockquote-bg, #21262d);
  }

  /* ========== 输入面板：永久白色卡片 ========== */
  #waline .wl-panel {
    position: relative;
    background: #ffffff !important;
    --waline-bgcolor: #ffffff;
    --waline-color: #333333;
    --waline-border-color: #e5e7eb;
    --waline-bgcolor-light: #f9fafb;
    --waline-info-color: #6b7280;
    --waline-theme-color: #000000;
    --waline-active-color: #333333;
    border-radius: 12px;
    box-shadow: 0 2px 12px rgba(0, 0, 0, 0.06);
  }

  /* 输入框聚焦不变灰 */
  #waline .wl-input,
  #waline .wl-editor {
    border-radius: 0.375rem;
    transition: border-color 0.2s ease, box-shadow 0.2s ease;
    background: #ffffff !important;
  }
  #waline .wl-input:focus,
  #waline .wl-editor:focus {
    border-color: #000000;
    box-shadow: 0 0 0 0.2rem rgba(0, 0, 0, 0.1);
    background: #ffffff !important;
  }

  /* 提交按钮黑底白字 */
  #waline .wl-btn {
    border-radius: 8px;
    font-weight: 500;
  }
  #waline .wl-btn.primary {
    background-color: #000000 !important;
    color: #ffffff !important;
    border: none !important;
  }
  #waline .wl-btn.primary:hover {
    background-color: #222222 !important;
  }

  /* ========== 右下角背景图 ========== */
  #waline .wl-panel::after {
    content: '';
    position: absolute;
    right: 24px;
    bottom: 30px;
    width: 190px;
    height: auto;
    aspect-ratio: 1 / 0.9;
    background-image: url('https://cdn.jsdmirror.com/gh/2478800972/boke@main/assets/img/da.png');
    background-size: contain;
    background-repeat: no-repeat;
    background-position: right bottom;
    opacity: 0.9;
    pointer-events: none;
    z-index: 2;
    transition: opacity 0.3s ease;
    border-bottom-right-radius: 12px;
  }

  #waline .wl-header,
  #waline .wl-editor,
  #waline .wl-footer {
    position: relative;
    z-index: 1;
  }

  #waline .wl-card {
    padding-bottom: 1rem;
    margin-bottom: 1rem;
  }

  /* ========== 表情/GIF弹窗：最高层级，不会被遮挡 ========== */
  #waline .wl-emoji-popup,
  #waline .wl-gif-popup {
    border-radius: 0.375rem;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.08);
    z-index: 99 !important;
  }

  /* 移动端适配 */
  @media (max-width: 576px) {
    #waline { --waline-avatar-size: 2.5rem; }
    #waline .wl-panel::after { width: 120px; right: 12px; bottom: 20px; }
    #waline .wl-panel { border-radius: 10px; }
  }

  /* ========== 隐藏版本号和版权信息 ========== */
  #waline .wl-power {
    display: none !important;
  }
</style>

<!-- 关键改动：使用 UMD 格式，而非 type="module" -->
<script src="https://unpkg.com/@waline/client@v3/dist/waline.umd.js"></script>

<script>
  document.addEventListener('DOMContentLoaded', function() {
    var container = document.querySelector('#waline');
    if (!container) {
      console.warn('未找到 #waline 容器，可能评论被禁用或页面未加载完成。');
      return;
    }

    try {
      Waline.init({
        el: '#waline',
        serverURL: '自己部署的Waline 服务端地址',
        
/* 统一路径：去掉开头的 /boke，使 GitHub Pages（仓库名为 boke）与其他站点的评论 path 一致，实现数据互通，原版为path: window.location.pathname, */

        path: window.location.pathname.replace(/^\/boke/, ''),
        dark: 'html[data-mode="dark"]',
        lang: 'zh-CN',
        meta: ['nick', 'mail', 'link'],
        requiredMeta: [],
        login: 'disable',
        wordLimit: 500,
        pageSize: 10,
        copyright: false,
        commentSorting: 'latest',
        search: false,
        locale: {
          placeholder: '你可以在这里输入评论内容... 但不要乱输入',
          submit: '来一发吗？',
          optional: '可选',
          nick: '昵称',
          mail: '邮箱',
          link: '网址'
        }
      });
      console.log('✅ Waline 初始化成功 (UMD 模式)');
    } catch (err) {
      console.error('❌ Waline 初始化失败：', err);
    }
  });
</script>
{% endif %}
```

---

### 3.2 方案二：ESM 版本（ES Module）

特征：使用 `<script type="module">` 加载，调用方式为 import { init } from '...' 然后 init({ ... })。

修改要点：同样在 init({}) 配置对象中，添加 path 选项。

完整代码示例（ESM 版）：

```html
{% if page.comments %}
<!-- Waline 评论区 - 黑色主题 + 明暗适配 + 移动端防遮挡 + 背景图置顶 -->
<section class="mt-5 pt-4 border-top">
  <h4 class="mb-4 fw-medium">{{ site.data.locales[lang].comment.comments }}</h4>
  <div id="waline"></div>
</section>

<link rel="stylesheet" href="https://unpkg.com/@waline/client@v3/dist/waline.css" />
<style>
  /* ========== 全局主题：黑色系 ========== */
  :root {
    --waline-theme-color: #000000;
    --waline-active-color: #333333;
    --waline-color: var(--text-color, #333);
    --waline-bgcolor: var(--body-bg, #fff);
    --waline-bgcolor-light: var(--card-bg, #f8f9fa);
    --waline-border-color: var(--border-color, #e9ecef);
    --waline-code-bgcolor: var(--code-bg, #f6f8fa);
    --waline-info-color: var(--text-muted, #6c757d);
    --waline-bq-color: var(--blockquote-bg, #f0f0f0);
    --waline-avatar-radius: 50%;
    --waline-font-size: 0.9375rem;
  }

  html[data-mode="dark"] {
    --waline-theme-color: #ffffff;
    --waline-active-color: #dddddd;
    --waline-color: var(--text-color, #c9d1d9);
    --waline-bgcolor: var(--body-bg, #0d1117);
    --waline-bgcolor-light: var(--card-bg, #161b22);
    --waline-border-color: var(--border-color, #30363d);
    --waline-code-bgcolor: var(--code-bg, #161b22);
    --waline-info-color: var(--text-muted, #8b949e);
    --waline-bq-color: var(--blockquote-bg, #21262d);
  }

  /* ========== 输入面板：永久白色卡片 ========== */
  #waline .wl-panel {
    position: relative;
    background: #ffffff !important;
    --waline-bgcolor: #ffffff;
    --waline-color: #333333;
    --waline-border-color: #e5e7eb;
    --waline-bgcolor-light: #f9fafb;
    --waline-info-color: #6b7280;
    --waline-theme-color: #000000;
    --waline-active-color: #333333;
    border-radius: 12px;
    box-shadow: 0 2px 12px rgba(0, 0, 0, 0.06);
  }

  /* 输入框聚焦不变灰 */
  #waline .wl-input,
  #waline .wl-editor {
    border-radius: 0.375rem;
    transition: border-color 0.2s ease, box-shadow 0.2s ease;
    background: #ffffff !important;
  }
  #waline .wl-input:focus,
  #waline .wl-editor:focus {
    border-color: #000000;
    box-shadow: 0 0 0 0.2rem rgba(0, 0, 0, 0.1);
    background: #ffffff !important;
  }

  /* 提交按钮黑底白字 */
  #waline .wl-btn {
    border-radius: 8px;
    font-weight: 500;
  }
  #waline .wl-btn.primary {
    background-color: #000000 !important;
    color: #ffffff !important;
    border: none !important;
  }
  #waline .wl-btn.primary:hover {
    background-color: #222222 !important;
  }

  /* ========== 右下角背景图 ========== */
  #waline .wl-panel::after {
    content: '';
    position: absolute;
    right: 24px;
    bottom: 30px;
    width: 190px;
    height: auto;
    aspect-ratio: 1 / 0.9;
    background-image: url('https://cdn.jsdmirror.com/gh/2478800972/boke@main/assets/img/da.png');
    background-size: contain;
    background-repeat: no-repeat;
    background-position: right bottom;
    opacity: 0.9;
    pointer-events: none;
    z-index: 2;
    transition: opacity 0.3s ease;
    border-bottom-right-radius: 12px;
  }

  #waline .wl-header,
  #waline .wl-editor,
  #waline .wl-footer {
    position: relative;
    z-index: 1;
  }

  #waline .wl-card {
    padding-bottom: 1rem;
    margin-bottom: 1rem;
  }

  /* ========== 表情/GIF弹窗：最高层级，不会被遮挡 ========== */
  #waline .wl-emoji-popup,
  #waline .wl-gif-popup {
    border-radius: 0.375rem;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.08);
    z-index: 99 !important;
  }

  /* 移动端适配 */
  @media (max-width: 576px) {
    #waline { --waline-avatar-size: 2.5rem; }
    #waline .wl-panel::after { width: 120px; right: 12px; bottom: 20px; }
    #waline .wl-panel { border-radius: 10px; }
  }
</style>

<script type="module">
  import { init } from 'https://unpkg.com/@waline/client@v3/dist/waline.js';
  init({
    el: '#waline',
    serverURL: '自己部署的Waline 服务端地址',
    /* 统一路径：去掉开头的 /boke，使 GitHub Pages（仓库名为 boke）与其他站点的评论 path 一致，实现数据互通 */
    path: window.location.pathname.replace(/^\/boke/, ''),
    dark: 'html[data-mode="dark"]',
    lang: 'zh-CN',
    meta: ['nick', 'mail', 'link'],
    requiredMeta: [],
    login: 'disable',
    wordLimit: 500,
    pageSize: 10,
    copyright: false,
    commentSorting: 'latest',
    search: false, // 禁用GIF搜索，解决转圈问题
    locale: {
      placeholder: '你可以在这里输入评论内容... 但不要乱输入',
      submit: '来一发吗？',
      optional: '可选',
      nick: '昵称',
      mail: '邮箱',
      link: '网址'
    }
  });

  // 移动端软键盘弹出自动上移
  const walineBox = document.querySelector('#waline');
  function scrollToInput() {
    setTimeout(() => {
      walineBox.querySelector('.wl-panel').scrollIntoView({
        behavior: 'smooth',
        block: 'end'
      });
    }, 350);
  }
  walineBox.addEventListener('focusin', scrollToInput);
  if (window.visualViewport) {
    window.visualViewport.addEventListener('resize', scrollToInput);
  }
</script>
{% endif %}
```

### 3.3 关键点说明

| 配置项 | 说明 |
|--------|------|
| path: window.location.pathname.replace(/^\/boke/, '') | 去掉路径开头的 /boke，使所有站点提交的 path 统一为 /posts/wz/ |
| serverURL: '自己部署的Waline 服务端地址' | Waline 服务端地址 |
| login: 'disable' | 关闭强制登录，允许游客评论 |
| 注释写法 | 必须用 /* */ 多行注释，不要用 // 放在配置项内部，否则会破坏 JavaScript 语法 |

## ✅ 验证方法

### 1. 验证 SECURE_DOMAINS 是否生效

#### 方法一：通过博客前端请求验证（推荐）

1. 打开你的博客文章页面（如 https://momoboke.cc.cd/posts/wz/）
2. 按 F12 打开开发者工具，切换到 Network（网络） 标签
3. 刷新页面，在请求列表中找到发往 自己部署的Waline 服务端地址/comment 的请求
4. 查看该请求的 状态码（Status）：
   - 200：请求成功，说明 SECURE_DOMAINS 配置正确（或已删除/未配置）
   - 403：请求被拒绝，说明当前博客域名不在白名单中

#### 方法二：通过浏览器地址栏直接访问（辅助验证）

- 直接访问 自己部署的Waline 服务端地址/comment?path=/posts/wz/
- 如果返回 403 Forbidden：说明 SECURE_DOMAINS 生效了（地址栏请求没有 Referer，被拦截）
- 如果返回 200 或 JSON 数据：说明 SECURE_DOMAINS 可能未配置或已删除

> ⚠️ 注意：方法二仅供参考。直接从地址栏访问时，请求头中可能缺少 Referer 信息，不能完全代表博客前端请求的真实状态。建议以方法一的判断为准。

### 2. 确认 path 统一

在浏览器控制台执行：

```javascript
console.log(window.location.pathname.replace(/^\/boke/, ''));
```

- GitHub Pages 页面应输出 /posts/wz/
- 自定义域名页面应输出 /posts/wz/（无变化）

### 3. 验证评论互通

1. 在 momoboke.cc.cd 的某篇文章下发一条新评论
2. 刷新 2478800972.github.io 的同篇文章
3. 评论显示即表示互通成功

## ⚠️ 常见问题与解决

| 问题 | 原因 | 解决方法 |
|------|------|----------|
| 后台登录报"账号密码错误" | SECURE_DOMAINS 未包含 waline.cc.cd | 添加 waline.cc.cd 并重新部署 |
| 修改代码后评论框消失 | // 注释位置不当导致语法错误 | 改用 /* */ 多行注释 |
| Script error | 跨域脚本报错，浏览器隐藏了详细信息 | 检查 Waline.init / init 配置是否有语法错误 |
| 数据库 UPDATE 报错 column "path" does not exist | 字段名是 url 而非 path | 使用 url 字段名 |
| 数据库 UPDATE 报错 relation "wl_comments" does not exist | 表名是 wl_comment（单数） | 使用 wl_comment 表名 |

## 📋 核心配置总结

| 配置项 | 值 | 作用 |
|--------|----|------|
| SECURE_DOMAINS | 2478800972.github.io,momoboke.cc.cd,boke-9u8.pages.dev,waline.cc.cd | 允许这些域名访问 Waline API |
| WALINE_ADMIN_EMAIL | 你的邮箱 | 管理员登录账号（临时） |
| WALINE_ADMIN_PASSWORD | 你的密码 | 管理员登录密码（临时） |
| path | window.location.pathname.replace(/^\/boke/, '') | 统一所有站点的文章路径 |

## 🔧 后续优化：清理 SECURE_DOMAINS

配置完成后，SECURE_DOMAINS 环境变量可以安全删除，不影响评论功能和后台登录。

| 问题 | 回答 |
|------|------|
| 删除后有什么影响？ | Waline 将允许所有来源的请求（即放开跨域限制），现有评论、登录、数据存储均不受影响 |
| 有什么风险？ | 理论上可能增加垃圾评论风险，但评论审核机制可防御，通常可忽略 |
| 建议保留还是删除？ | 如果担心安全，可以保留；如果为了简化配置，可以删除 |
| 删除后需要做什么？ | 在 Vercel 中删除该变量后，重新部署（Redeploy） 即可 |

## 🔄 Waline 评论系统后续更新指南

Waline 分为前端和后端两部分，更新方式不同。

### 前端（客户端）更新

前端即博客中加载的 Waline JS 脚本，目前使用 CDN 地址：

```
https://unpkg.com/@waline/client@v3/dist/waline.umd.js   # UMD 版
https://unpkg.com/@waline/client@v3/dist/waline.js      # ESM 版
```

#### 更新方法：

1. 查看 Waline 最新版本：npm 页面
2. 修改 CDN 链接中的版本号，例如：
   - @v3 → @v4（大版本升级）
   - @v3.1.0 → @v3.2.0（小版本升级）
3. 保存并重新部署博客

```html
<!-- 示例：锁定具体版本（UMD） -->
<script src="https://unpkg.com/@waline/client@3.2.0/dist/waline.umd.js"></script>
<!-- 或 ESM 版本 -->
<script type="module">
  import { init } from 'https://unpkg.com/@waline/client@3.2.0/dist/waline.js';
</script>
```

### 后端（服务端）更新

后端即部署在 Vercel 上的 @waline/vercel 服务。

#### 更新步骤：

1. 登录 Vercel，进入 waline 项目关联的 GitHub 仓库
2. 找到 package.json 文件并编辑
3. 找到 "@waline/vercel" 依赖，修改版本号为最新版本
4. 提交修改，Vercel 会自动触发重新部署

```json
{
  "dependencies": {
    "@waline/vercel": "最新版本号"
  }
}
```

#### 升级特别提醒：

| 注意事项 | 说明 |
|----------|------|
| Node.js 版本 | Waline V3 要求 Node.js 18 或 20 及以上版本，请在 Vercel 项目设置中确认 |
| 配置备份 | 如果修改过 index.js 等核心文件，更新前请先备份，以防被覆盖 |
| 数据库迁移 | 大版本升级（如 V2 → V3）可能需要执行数据库迁移，请查阅 Waline 官方文档 |
| 测试环境 | 建议先在预览环境（Preview）测试，确认无误后再部署到生产环境（Production） |

## 🔗 相关链接

- Waline 官网：https://waline.js.org/
- Waline GitHub：https://github.com/walinejs/waline
- Vercel 控制台：https://vercel.com
- Neon 控制台：https://console.neon.tech
- bcrypt 在线生成器：https://bcrypt-generator.com/（Cost 选 8）
- @waline/client npm：https://www.npmjs.com/package/@waline/client
- @waline/vercel npm：https://www.npmjs.com/package/@waline/vercel
