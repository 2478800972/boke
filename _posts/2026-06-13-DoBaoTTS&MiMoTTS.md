---
title: DoBaoTTS & MiMoTTS推文
description: 开源阅读高品质听书方案合集
author: 墨殇
date: 2026-06-13 08:00:00 +0800
categories: [开源软件, DoBaoTTS&MiMoTTS]
tags: [TTS, 引擎, 自定义]
pin:                 # true 表示置顶，false 或不写则不置顶
math: true                # 启用数学公式（需要时设为 true）
mermaid: true             # 启用流程图（需要时设为 true）
---

# DoBaoTTS & MiMoTTS 项目分享 | 开源阅读高品质听书方案合集

> 哔哩哔哩@夜雨扰心弦 出品 · 适配开源阅读生态的多款TTS听书工具

## 一、DoBaoTTS 豆包TTS听书项目
### 项目简介
DoBaoTTS 是一款适配开源阅读APP朗读引擎的豆包语音朗读逆向调用项目，可让你在开源阅读及各类二改版本中，使用豆包官方全量音色来朗读小说内容。

- B站官方作者：**夜雨扰心弦**
- 酷安技术分享作者：**郁莲仰慕辰**

### 核心能力
- **全音色覆盖**：完整支持豆包官方全部发音人
- **可视化配置**：自带WebUI管理界面，支持图形化调节各项参数
- **多Cookie轮询**：内置多账号凭证轮询机制，降低高频调用带来的封禁风险
- **多端部署**：支持 Windows 10/11、Android Termux 双平台部署
- **高兼容性**：完美适配原版开源阅读与各类二改客户端
- **语音导出**：支持基础文本转语音，可生成并下载语音文件

### 官方下载渠道
项目安装包可通过以下网盘渠道获取，后续版本更新均沿用该链接：
- 夸克盘：https://pan.quark.cn/s/bec6b4aa62b8
- 百度盘：https://pan.baidu.com/s/15Xxj3xaXZn6i2rjcWvU2Sw?pwd=e4n7



### 两种听书模式
#### 实时模式
逐句发送文本、逐句返回合成语音，随听随生成。
- ✅ 优点：无需提前预处理，支持自由调节朗读进度、实时调整语速
- ❌ 缺点：单句单次请求，调用频率高，Cookie数量较少时极易触发封禁，当前逻辑仍在优化中

#### 预制书模式
提前批量处理整本书内容，生成完整语音集后本地调用。
- ✅ 优点：通过控制请求间隔大幅降低封禁风险，支持朗读进度调节
- ❌ 缺点：需提前预制完成才可收听，不支持实时调速；更换书源、调整阅读净化/布局可能导致语音匹配失效

> 💡 使用参考
> - 持有5个Cookie：可设置15秒左右低延迟，预制数章后即可边听边生成
> - 仅1个Cookie：建议设置25秒左右高延迟，在闲置时段提前预制
>
> 注：Cookie封禁通常持续约半小时后会自行解封

### 重要免责声明
1. 本项目仅用于**技术学习与交流**，严禁商用
2. 项目暂不开源，获取后请于24小时内自行删除
3. 使用本项目导致的账号封禁、法律责任等一切后果，均由使用者自行承担
4. 长期稳定使用，建议前往火山引擎官方正规渠道购买TTS API服务

## 二、MiMo-V2.5-TTS 小米TTS听书项目
### 项目简介
同作者出品的另一款开源阅读听书方案，基于小米免费MIMO-TTS语音引擎定制，适配阅读APP朗读引擎调用逻辑，支持多发音人与音色克隆功能。

### 核心特性
- 已更新至 **V2.5** 版本，合成效果与稳定性提升
- 支持**音色克隆**功能，可定制专属朗读音色
- 内置海量发音人，覆盖多种风格与语种
- 适配原版开源阅读及各类二改客户端
- 听书流畅度高，整体体验接近真人朗读

## 三、一些MiMoTTS项目
社区开发者基于相关TTS方案制作的衍生版本，扩展了多平台部署与使用场景：

### 1. DoBaoTTS Docker 版
- 制作作者：哔哩哔哩@何必丶太在乎（UID：24671850）
- 分享地址：https://pan.quark.cn/s/0e8cbe548522


### 2. MiMoTTS 安卓版
- 制作作者：哔哩哔哩@蒸_不_戳（UID：18928796）
- 项目简介：通过 MiMo TTS v2.5 API 为开源阅读APP（Legado）提供朗读引擎的 Android 应用。

✨ 功能特性
🎙️ MiMo TTS v2.5 全功能支持 - 预置音色、音色设计、音色复刻三种模型
📖 Legado 阅读APP 集成 - 一键导入朗读规则，无缝对接开源阅读
🎵 9种预置音色 - 冰糖、茉莉、苏打、白桦、Mia、Chloe、Milo、Dean
🗣️ 方言支持 - 东北话、四川话、河南话、粤语
🎭 风格控制 - 支持情感标签（开心/悲伤/温柔/高冷/唱歌等）和自然语言指令
🔄 流式/非流式合成 - 支持两种 API 调用模式
🛡️ 服务保活 - 前台服务 + WakeLock + 电池优化白名单
🚀 开机自启 - 可选开机自动启动 TTS 服务
📱 纯原生架构 - 无需 Node.js 运行时，轻量高效
- 项目地址：https://github.com/timyang2005/MiMoTTSReader


### 3. VoxEngine 系统级 TTS 引擎
- 制作作者：GitHub@Autsunset
- 项目简介：Android 系统级 TTS 语音合成引擎，支持多引擎切换、音色克隆与设计。注册为系统 TTS 服务后，任意支持系统语音合成的应用（如 Legado 阅读器）均可直接调用。
- 项目地址：https://github.com/Autsunset/VoxEngine


### 4. MiMoTTS Vercel 部署版
- 制作作者：GitHub@ISuuuu
- 项目简介：为 legado 阅读 App 提供小米 MiMo TTS 语音合成服务。
- 项目地址：https://github.com/ISuuuu/mitts


### 5. MiMoTTS 桌面工具
- 制作作者：GitHub@jarodise
- 项目简介：基于 小米 MiMo TTS API 的桌面语音合成工具，支持预置音色、文本设计音色、音色复刻三种模式。
- 功能
预置音色：内置冰糖、茉莉、苏打等精品音色，支持风格指令控制
文本设计音色：通过文字描述自定义音色
音色复刻：上传音频样本或选择预置音色克隆，支持风格指令
合成结果自动播放，支持 WAV / MP3 格式输出
- 项目地址：https://github.com/jarodise/MimoTTS

## 相关地址
### 官方项目相关
#### DoBaoTTS
- 🎬 B站项目介绍教程：[DoBaoTTS项目正式介绍与教程](https://www.bilibili.com/video/BV1BcSDBbEwf)

<div style="position: relative; width: 100%; padding-bottom: 56.25%; /* 16:9 比例 = 9/16 = 0.5625 */">
<iframe style="position: absolute; top: 0; left: 0; width: 100%; height: 100%;" src="https://player.bilibili.com/player.html?bvid=BV1BcSDBbEwf&page=1" title="Bilibili video" frameborder="0" allowfullscreen>
</iframe>
</div>

- 🎬 B站资源分享视频：[项目下载与更新说明](https://www.bilibili.com/video/BV1uCdfBWEng)

<div style="position: relative; width: 100%; padding-bottom: 56.25%; /* 16:9 比例 = 9/16 = 0.5625 */">
<iframe style="position: absolute; top: 0; left: 0; width: 100%; height: 100%;" src="https://player.bilibili.com/player.html?bvid=BV1uCdfBWEng&page=1" title="Bilibili video" frameborder="0" allowfullscreen> 
</iframe>
</div>

#### MiMo-TTS
- 🎬 B站版本更新视频：[MIMO-V2.5-TTS 听书方案更新](https://www.bilibili.com/video/BV1VUozBGEyf)

<div style="position: relative; width: 100%; padding-bottom: 56.25%; /* 16:9 比例 = 9/16 = 0.5625 */">
<iframe style="position: absolute; top: 0; left: 0; width: 100%; height: 100%;" src="https://player.bilibili.com/player.html?bvid=BV1VUozBGEyf&page=1" title="Bilibili video" frameborder="0" allowfullscreen>
 </iframe>
</div>


---

- 📱 酷安详细分享：[郁莲仰慕辰 项目动态](https://www.coolapk.com/feed/70200482?s=ZmRmNzZiMDYzM2E5MWRnNmEyZDJkOGJ6a1631)

---


### 作者主页
- 🏠 B站作者主页：[夜雨扰心弦](https://space.bilibili.com/3706943998789966)
- 🏠 酷安作者主页：[郁莲仰慕辰](https://www.coolapk.com/u/3379301)
