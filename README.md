# 💕 恋爱纪念册 (Love Diary)

一个专为情侣设计的私密云端纪念册，记录两人之间每一个珍贵的瞬间。

![License](https://img.shields.io/badge/license-MIT-pink)
![HTML](https://img.shields.io/badge/HTML-5-ff69b4)
![Supabase](https://img.shields.io/badge/Backend-Supabase-green)
![Cloudinary](https://img.shields.io/badge/Image-Cloudinary-blue)

---

## ✨ 功能特色

- 📝 **发布动态** — 写下心情、记录日常，支持自定义时间
- 📷 **多图上传** — 上传多张照片，自动适配单图/双图/多图布局
- 💬 **互动评论** — 支持对动态发表评论，并可回复他人评论
- 🖼️ **图片查看** — 点击任意图片全屏查看
- ☁️ **云端同步** — 基于 Supabase，所有设备实时同步数据
- 📱 **移动适配** — 完整响应式设计，手机体验流畅
- 🔐 **账号系统** — 简单的用户名/密码登录，首次使用自动注册

---

## 🛠️ 技术栈

| 层级 | 技术 |
|------|------|
| 前端 | 原生 HTML / CSS / JavaScript（单文件，零依赖框架）|
| 数据库 | [Supabase](https://supabase.com)（PostgreSQL 云数据库）|
| 图床 | [Cloudinary](https://cloudinary.com)（推荐）或 [ImgBB](https://imgbb.com) |
| 部署 | 任意静态托管平台（GitHub Pages / Vercel / Netlify 等）|

---

## 📁 项目结构

```
Love_dairy/
├── index.html                  # 主版本（ImgBB 图床）
├── love-diary-cloudinary.html  # Cloudinary 图床版本（推荐）
└── README.md
```

> **推荐使用** `love-diary-cloudinary.html`，Cloudinary 提供更稳定的图片存储服务（免费 25GB 流量 + 10GB 存储）。

---

## 🚀 快速开始

### 第一步：配置 Supabase 数据库

1. 前往 [supabase.com](https://supabase.com) 免费注册并创建新项目
2. 在 **SQL Editor** 中执行以下建表语句：

```sql
-- 动态表
CREATE TABLE posts (
  id bigint generated always as identity primary key,
  post_id bigint unique not null,
  author text not null,
  content text,
  images text[],
  date timestamptz default now()
);

-- 评论表
CREATE TABLE comments (
  id bigint generated always as identity primary key,
  post_id bigint references posts(post_id) on delete cascade,
  author text not null,
  content text,
  images text[],
  reply_to text,
  date timestamptz default now()
);

-- 开放读写权限（适合私人使用）
ALTER TABLE posts ENABLE ROW LEVEL SECURITY;
ALTER TABLE comments ENABLE ROW LEVEL SECURITY;

CREATE POLICY "allow all" ON posts FOR ALL USING (true) WITH CHECK (true);
CREATE POLICY "allow all" ON comments FOR ALL USING (true) WITH CHECK (true);
```

3. 在 **Settings → API** 中获取：
   - `Project URL`（即 Supabase URL）
   - `anon public` Key

### 第二步：配置 Cloudinary 图床

1. 前往 [cloudinary.com](https://cloudinary.com) 免费注册
2. 在 Dashboard 找到你的 **Cloud Name**
3. 前往 **Settings → Upload → Upload presets**，新建一个预设：
   - Signing Mode 选择 `Unsigned`
   - 保存后记录预设名称（Upload Preset）

### 第三步：部署与使用

将 `love-diary-cloudinary.html` 部署到任意静态托管服务，或直接用浏览器打开本地文件。

**首次使用：**
1. 输入用户名和密码（首次输入即自动注册）
2. 点击 **设置**，填入 Supabase URL、Key 以及 Cloudinary 配置
3. 保存后即可开始记录

---

## 📸 界面预览

```
┌─────────────────────────────┐
│  💕 恋爱纪念册   [设置][退出] │
├─────────────────────────────┤
│  ┌───────────────────────┐  │
│  │ 写下你想说的话...      │  │
│  │                       │  │
│  └───────────────────────┘  │
│  [日期时间]  [📷 添加照片] [发布] │
├─────────────────────────────┤
│  ❤️ 小明                     │
│  今天我们去了海边，好开心～     │
│  [图片] [图片]               │
│  💬 评论 (2)                 │
└─────────────────────────────┘
```

---

## ⚙️ 配置说明

所有配置信息保存在浏览器的 `localStorage` 中，不会上传到任何服务器。

| 配置项 | 说明 | 获取方式 |
|--------|------|----------|
| Supabase URL | 数据库地址 | Supabase → Settings → API |
| Supabase Anon Key | 数据库访问密钥 | Supabase → Settings → API |
| Cloudinary Cloud Name | 云名称 | Cloudinary Dashboard |
| Cloudinary Upload Preset | 上传预设名称 | Cloudinary → Settings → Upload |

---

## 🔒 安全说明

- 用户密码以明文存储在浏览器 `localStorage`，**仅适合私人/信任环境使用**
- Supabase 默认开启了"允许所有"的 RLS 策略，如需更严格的权限控制，可在 Supabase 中自定义 Row Level Security 规则
- 建议将本项目仅分享给信任的伴侣，不建议公开部署

---

## 🌟 版本说明

| 文件 | 图床服务 | 推荐指数 |
|------|----------|----------|
| `love-diary-cloudinary.html` | Cloudinary | ⭐⭐⭐⭐⭐ 推荐 |
| `index.html` | ImgBB | ⭐⭐⭐ |

Cloudinary 版本稳定性更高，支持全球 CDN 加速，图片永久保存，免费额度对日常使用完全够用。

---

## 📄 License

[MIT](LICENSE) © 2024

---

> 💌 愿每一对恋人都能用这个小册子，留住属于彼此的每一个美好瞬间。
