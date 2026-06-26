---
title: Waline 评论系统 Vercel 部署教程
description: Waline 是一款基于 Valine 衍生的简洁、安全的评论系统，支持浏览量统计。通过 Vercel 部署服务端，再配合 Neon 数据库，即可快速搭建属于自己的评论系统。以下是完整的部署步骤。
author: 墨殇
date: 2026-06-26 20:00:00 +0800
categories: [教程, 技术]
tags: [Chirpy, Waline, 功能]
pin:                 # true 表示置顶，false 或不写则不置顶
math: true                # 启用数学公式（需要时设为 true）
mermaid: true             # 启用流程图（需要时设为 true）
image:
  path: https://cdn.jsdmirror.com/gh/2478800972/boke@main/assets/img/tw/wz/绣球花2.png      # 预览图路径（注意加上 /boke 前缀）
  alt: 美丽的绣球花•͈ᴗ⁃͈ ✧
comments: true
---


# Waline 评论系统 Vercel 部署教程

Waline 是一款基于 Valine 衍生的简洁、安全的评论系统，支持浏览量统计。通过 Vercel 部署服务端，再配合 Neon 数据库，即可快速搭建属于自己的评论系统。以下是完整的部署步骤。

---

## 一、部署服务端

1. **点击部署按钮**：访问 Waline Vercel 部署页面，点击页面中的部署按钮，跳转至 Vercel 进行服务端部署。
2. **登录 Vercel**：如果未登录，Vercel 会提示注册或登录，建议使用 GitHub 账户进行快捷登录。
3. **创建项目**：输入一个你喜欢的 Vercel 项目名称，点击 **Create** 继续。
4. **等待部署完成**：Vercel 会基于 Waline 模板自动创建并初始化仓库，仓库名即你输入的项目名。一两分钟后部署完成，点击 **Go to Dashboard** 可进入应用控制台。

---

## 二、创建数据库（Neon）

1. **进入 Storage 页面**：在 Vercel 项目控制台顶部点击 **Storage**，进入存储服务配置页。
2. **创建数据库**：选择 **Create Database**，在 Marketplace Database Providers 中选 **Neon**，点击 **Continue**。
3. **创建 Neon 账号**：按提示创建 Neon 账号，选择 **Accept and Create**。
4. **选择套餐**：选择数据库套餐配置（地区、额度等），可直接使用默认配置，点击 **Continue**。
5. **命名数据库**：定义数据库名称，可直接使用默认名称，点击 **Continue**。
6. **执行 SQL 建表**：在 Storage 中点击刚创建的数据库服务，选择 **Open in Neon** 跳转至 Neon。在 Neon 左侧选择 **SQL Editor**，将 `waline.pgsql` 中的 SQL 语句粘贴到编辑器中，点击 **Run** 执行建表操作。稍等片刻会提示创建成功。

---

## 三、重新部署使数据库生效

1. 回到 Vercel，点击顶部的 **Deployments**。
2. 点击最新一次部署右侧的 **Redeploy** 按钮，重新部署。
3. 等待部署完成，STATUS 变为 **Ready** 后，点击 **Visit** 即可访问部署好的网站，此地址即为你的服务端地址（即 `serverURL`）。

---

## 四、绑定自定义域名（必选Vercel分配的域名国内屏蔽了）

1. 在 Vercel 项目控制台点击 **Settings → Domains** 进入域名配置页。
2. 输入需要绑定的域名，点击 **Add**。
3. 在域名服务商处添加 CNAME 解析记录：

| 类型 | 名称 | 值 |
|------|------|-----|
| CNAME | 你的域名 | cname.vercel-dns.com |

4. 等待解析生效后，即可通过自己的域名访问：
   - 评论系统：`example.yourdomain.com`
   - 评论管理后台：`example.yourdomain.com/ui`

---

## 五、环境变量配置（可选）

Waline 支持通过环境变量进行服务端配置，在 Vercel 的 **Settings → Environment Variables** 中设置。常用环境变量包括：

| 变量名 | 说明 |
|--------|------|
| `SITE_NAME` | 博客名称 |
| `SITE_URL` | 博客地址 |
| `LOGIN` | 设为 `force` 时强制登录才能评论 |
| `SERVER_URL` | Waline 服务端地址 |
| `COMMENT_AUDIT` | 设为 `true` 开启评论审核 |
| `SECURE_DOMAINS` | 安全域名，支持逗号分隔多个 |

> ⚠️ **注意**：环境变量更新后必须重新部署才能生效。

---

## 六、前端引入 Waline

在你的网页中添加以下代码：

```html
<head>
  <!-- 导入 Waline 样式 -->
  <link rel="stylesheet" href="https://unpkg.com/@waline/client@v3/dist/waline.css" />
</head>
<body>
  <!-- 评论容器 -->
  <div id="waline"></div>

  <script type="module">
    import { init } from 'https://unpkg.com/@waline/client@v3/dist/waline.js';
    init({
      el: '#waline',
      serverURL: 'https://your-domain.vercel.app'  // 替换为你的服务端地址
    });
  </script>
</body>
```

- **el**：Waline 渲染的目标元素，可以是 CSS 选择器或 HTMLElement
- **serverURL**：服务端地址，即第三步获取的地址

---

## 七、评论管理（管理端）

1. 部署完成后，访问 `<serverURL>/ui/register` 进行注册。
2. 首个注册的用户会被自动设为管理员。
3. 管理员登录后即可在管理界面修改、标记或删除评论。
4. 普通用户也可通过评论框注册账号，登录后会跳转到个人档案页。

---

## ⚠️ 注意事项

1. **数据库选择**：官方推荐使用 Neon 作为数据库服务，部署流程最为顺畅。
2. **重新部署**：配置数据库或修改环境变量后，务必点击 **Redeploy** 重新部署才能使配置生效。
3. **版本说明**：本文基于 Waline Client v3 版本，请确保使用正确的 CDN 链接。

## 📺 视频教程

> 2026年！如何部署一个Waline评论系统？
>
> 🎬 [Waline部署教程](https://www.bilibili.com/video/BV1J2XuBtEGn)


<div style="position: relative; width: 100%; padding-bottom: 56.25%; /* 16:9 比例 = 9/16 = 0.5625 */">
<iframe style="position: absolute; top: 0; left: 0; width: 100%; height: 100%;" src="https://player.bilibili.com/player.html?bvid=BV1J2XuBtEGn&page=1" title="Bilibili video" frameborder="0" allowfullscreen>
 </iframe>
</div>

---

完成以上步骤后，你的 Waline 评论系统就正式上线了！🎉
