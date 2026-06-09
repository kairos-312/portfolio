# 盛欣格个人作品集网站 — 实现计划

> **适用执行方式：** 使用 superpowers:executing-plans（推荐）或 superpowers:subagent-driven-development 实现本计划。步骤使用 `- [ ]` 复选框跟踪进度。

**目标：** 搭建一个纯 HTML+CSS 的单页个人作品集网站，面向兼职客户展示 AI 设计能力。

**架构：** 单文件 `index.html`，内嵌 `<style>` 标签处理所有样式，响应式通过 `@media` 查询实现。所有图片从同级目录引用。

**技术栈：** 纯 HTML5 + CSS3，无框架、无 JavaScript 库。

---

## 文件结构

```
cc/
├── index.html          # 【新建】网站主文件，包含全部 HTML + CSS
├── 蓝底.jpg            # 【已有】证件照
├── 耳机广告-2.png      # 【已有】耳机广告
├── 反诈短剧宣传海报.png  # 【已有】反诈海报
├── 咖啡.png            # 【已有】咖啡海报
├── ai家居展览海报.png   # 【已有】AI家居展览
├── ai家居海报.png      # 【已有】AI家居
└── 酒店海报.png        # 【已有】酒店开业海报
```

---

### 任务 1：搭建 HTML 骨架 + 基础 CSS

**文件：**
- 新建：`index.html`

- [ ] **步骤 1：创建 `index.html` 并写入 HTML5 骨架 + 基础 CSS 变量**

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>盛欣格 — 全媒体设计师 · AI创作者</title>
  <style>
    /* ============================
       基础重置 + CSS 变量
       ============================ */
    :root {
      --cream: #f5f0e8;       /* 米白底色 */
      --white: #ffffff;
      --dark: #2d3436;        /* 深灰文字 */
      --gray: #636e72;        /* 次级灰色 */
      --orange: #e17055;      /* 橘红点缀 */
      --yellow: #fdcb6e;      /* 黄色点缀 */
      --green: #00b894;       /* 绿色点缀 */
      --blue: #0984e3;        /* 蓝色点缀 */
      --purple: #6c5ce7;      /* 紫色点缀 */
    }

    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }

    body {
      font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", "PingFang SC", "Microsoft YaHei", sans-serif;
      background: var(--white);
      color: var(--dark);
      line-height: 1.6;
    }

    /* 顶部彩虹条 */
    .top-stripe {
      height: 4px;
      background: linear-gradient(90deg, var(--orange), var(--yellow), var(--green), var(--blue), var(--purple));
    }

    /* 底部彩虹条 */
    .bottom-stripe {
      height: 3px;
      background: linear-gradient(90deg, var(--orange), var(--yellow), var(--green), var(--blue), var(--purple));
    }

    /* 内容容器 —— 手机端满宽，大屏居中 */
    .container {
      max-width: 960px;
      margin: 0 auto;
    }
  </style>
</head>
<body>

  <!-- 顶部彩虹条 -->
  <div class="top-stripe"></div>

  <!-- 所有板块内容将在此处添加 -->

  <!-- 底部彩虹条 -->
  <div class="bottom-stripe"></div>

</body>
</html>
```

- [ ] **步骤 2：浏览器打开测试**

用浏览器打开 `index.html`，确认页面空白但顶部/底部彩虹条可见、无报错。

- [ ] **步骤 3：Git 提交**

```bash
git add index.html
git commit -m "feat: 搭建网站骨架 + CSS变量 + 彩虹条"
```

---

### 任务 2：头部首屏（Hero）

**文件：**
- 修改：`index.html`

- [ ] **步骤 1：在 `<body>` 中彩虹条下方添加 Hero 区块**

```html
  <!-- ===== 头部首屏 ===== -->
  <header class="hero">
    <div class="container">
      <!-- 证件照：圆形裁切 -->
      <img class="avatar" src="蓝底.jpg" alt="盛欣格证件照">
      <!-- 展示名 -->
      <h1 class="hero-name">Sheng</h1>
      <p class="hero-realname">盛欣格</p>
      <!-- 身份标签 -->
      <p class="hero-tagline">全媒体专业 · AI创作者 · 北京交通大学</p>
      <!-- 联系方式 -->
      <div class="hero-contact">
        <span>📧 2643105249@qq.com</span>
        <span>📱 18299125958</span>
      </div>
    </div>
  </header>
```

- [ ] **步骤 2：在 `<style>` 中添加 Hero 样式**

```css
    /* ===== 头部首屏 ===== */
    .hero {
      background: var(--cream);
      text-align: center;
      padding: 50px 20px 40px;
    }

    .avatar {
      width: 100px;
      height: 100px;
      border-radius: 50%;
      object-fit: cover;
      border: 3px solid var(--white);
      box-shadow: 0 4px 12px rgba(0,0,0,0.1);
      margin-bottom: 16px;
    }

    .hero-name {
      font-size: 36px;
      font-weight: bold;
      color: var(--dark);
      letter-spacing: 2px;
    }

    .hero-realname {
      font-size: 16px;
      color: var(--gray);
      margin-bottom: 12px;
    }

    .hero-tagline {
      font-size: 14px;
      color: var(--gray);
      margin-bottom: 18px;
    }

    .hero-contact {
      display: flex;
      gap: 14px;
      justify-content: center;
      flex-wrap: wrap;
      font-size: 12px;
    }

    .hero-contact span {
      background: rgba(225, 112, 85, 0.1);
      color: var(--orange);
      padding: 4px 12px;
      border-radius: 16px;
    }
```

- [ ] **步骤 3：浏览器刷新测试**

确认照片圆形显示、Sheng 大字在前、盛欣格小字在后、联系方式按钮可见。

- [ ] **步骤 4：Git 提交**

```bash
git add index.html
git commit -m "feat: 添加头部首屏 Hero 区块"
```

---

### 任务 3：技能标签板块

**文件：**
- 修改：`index.html`

- [ ] **步骤 1：在 Hero 下方添加技能区块 HTML**

```html
  <!-- ===== 技能标签 ===== -->
  <section class="skills">
    <div class="container">
      <h2 class="section-title" style="color: var(--orange);">◆ 技能工具</h2>
      <div class="skill-tags">
        <span>即梦 / Lovart</span>
        <span>Photoshop</span>
        <span>ChatGPT / Claude</span>
        <span class="highlight-tag" style="color: var(--orange);">Agent 智能体搭建</span>
        <span class="highlight-tag" style="color: var(--orange);">提示词工程</span>
        <span class="highlight-tag" style="color: var(--green);">AI 图文创作</span>
        <span class="highlight-tag" style="color: var(--blue);">文案撰写</span>
        <span class="highlight-tag" style="color: var(--purple);">新媒体运营</span>
      </div>
    </div>
  </section>
```

- [ ] **步骤 2：在 `<style>` 中添加技能区块样式**

```css
    /* ===== 技能标签 ===== */
    .skills {
      background: var(--white);
      padding: 30px 20px;
    }

    .section-title {
      font-size: 14px;
      font-weight: bold;
      margin-bottom: 14px;
    }

    .skill-tags {
      display: flex;
      flex-wrap: wrap;
      gap: 8px;
    }

    .skill-tags span {
      background: #f0f0f0;
      padding: 6px 14px;
      border-radius: 20px;
      font-size: 13px;
      color: var(--dark);
    }

    .skill-tags .highlight-tag {
      font-weight: 600;
      background: rgba(225, 112, 85, 0.08);
    }
```

- [ ] **步骤 3：浏览器刷新，确认技能标签排列整齐**

- [ ] **步骤 4：Git 提交**

```bash
git add index.html
git commit -m "feat: 添加技能标签板块"
```

---

### 任务 4：项目经历板块

**文件：**
- 修改：`index.html`

- [ ] **步骤 1：在技能区块下方添加项目经历 HTML**

```html
  <!-- ===== 项目经历 ===== -->
  <section class="experience">
    <div class="container">
      <h2 class="section-title" style="color: var(--green);">◆ 项目经历</h2>

      <!-- 经历条目 1 -->
      <div class="exp-item" style="border-left-color: rgba(0, 184, 148, 0.3);">
        <h3 class="exp-title">一日店长活动策划</h3>
        <p class="exp-detail">2023 暑期社会实践 · 独立策划 + 海报设计 + 文案撰写</p>
      </div>

      <!-- 经历条目 2 -->
      <div class="exp-item" style="border-left-color: rgba(9, 132, 227, 0.3);">
        <h3 class="exp-title">酒店开业宣传设计</h3>
        <p class="exp-detail">商业兼职项目 · 主视觉海报 + 房型介绍图 + 价格展示图</p>
      </div>

      <!-- 经历条目 3 -->
      <div class="exp-item" style="border-left-color: rgba(108, 92, 231, 0.3);">
        <h3 class="exp-title">校学生会组织部</h3>
        <p class="exp-detail">北京交通大学 · 海报设计 + PPT 制作 + 反诈短剧宣传</p>
      </div>
    </div>
  </section>
```

- [ ] **步骤 2：在 `<style>` 中添加经历区块样式**

```css
    /* ===== 项目经历 ===== */
    .experience {
      background: var(--cream);
      padding: 30px 20px;
    }

    .exp-item {
      border-left: 3px solid;
      padding-left: 14px;
      margin-bottom: 16px;
    }

    .exp-item:last-child {
      margin-bottom: 0;
    }

    .exp-title {
      font-size: 14px;
      font-weight: bold;
      color: var(--dark);
      margin-bottom: 4px;
    }

    .exp-detail {
      font-size: 12px;
      color: var(--gray);
      line-height: 1.5;
    }
```

- [ ] **步骤 3：浏览器刷新，确认经历条目左侧有彩色竖线**

- [ ] **步骤 4：Git 提交**

```bash
git add index.html
git commit -m "feat: 添加项目经历板块"
```

---

### 任务 5：作品展示板块

**文件：**
- 修改：`index.html`

- [ ] **步骤 1：在经历区块下方添加作品展示 HTML**

```html
  <!-- ===== 作品展示 ===== -->
  <section class="works">
    <div class="container">
      <h2 class="section-title" style="color: var(--yellow);">◆ 作品展示</h2>

      <div class="work-grid">
        <!-- AI家居展览海报 -->
        <div class="work-item">
          <img src="ai家居展览海报.png" alt="AI家居展览海报" loading="lazy">
          <span class="work-label">AI 家居展览</span>
        </div>
        <!-- AI家居海报 -->
        <div class="work-item">
          <img src="ai家居海报.png" alt="AI家居海报" loading="lazy">
          <span class="work-label">AI 家居海报</span>
        </div>
        <!-- 耳机广告 -->
        <div class="work-item">
          <img src="耳机广告-2.png" alt="耳机产品广告" loading="lazy">
          <span class="work-label">耳机广告</span>
        </div>
        <!-- 咖啡海报 -->
        <div class="work-item">
          <img src="咖啡.png" alt="咖啡饮品海报" loading="lazy">
          <span class="work-label">咖啡海报</span>
        </div>
        <!-- 酒店海报 -->
        <div class="work-item">
          <img src="酒店海报.png" alt="酒店开业海报" loading="lazy">
          <span class="work-label">酒店海报</span>
        </div>
        <!-- 反诈短剧海报 -->
        <div class="work-item">
          <img src="反诈短剧宣传海报.png" alt="反诈短剧宣传海报" loading="lazy">
          <span class="work-label">反诈宣传</span>
        </div>
      </div>

      <!-- 点击放大后的遮罩层（纯 CSS 实现，无需 JS） -->
      <!-- 此处先用简单展示，后续如需灯箱效果再加 -->
    </div>
  </section>
```

- [ ] **步骤 2：在 `<style>` 中添加作品展示样式**

```css
    /* ===== 作品展示 ===== */
    .works {
      background: var(--white);
      padding: 30px 20px;
    }

    .work-grid {
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      gap: 8px;
    }

    .work-item {
      position: relative;
      overflow: hidden;
      border-radius: 6px;
      cursor: pointer;
      transition: transform 0.2s;
    }

    .work-item:hover {
      transform: scale(1.03);
    }

    .work-item img {
      width: 100%;
      height: 120px;
      object-fit: cover;
      display: block;
    }

    .work-label {
      position: absolute;
      bottom: 0;
      left: 0;
      right: 0;
      background: rgba(0,0,0,0.55);
      color: #fff;
      font-size: 10px;
      text-align: center;
      padding: 4px 0;
    }
```

- [ ] **步骤 3：浏览器刷新，确认 6 张图 3 列排列、hover 放大效果**

- [ ] **步骤 4：Git 提交**

```bash
git add index.html
git commit -m "feat: 添加作品展示板块（3列网格）"
```

---

### 任务 6：关于我 + 底部联系

**文件：**
- 修改：`index.html`

- [ ] **步骤 1：添加"关于我"和"底部联系"区块 HTML**

```html
  <!-- ===== 关于我 ===== -->
  <section class="about">
    <div class="container">
      <h2 class="section-title" style="color: var(--blue);">◆ 关于我</h2>
      <ul class="about-list">
        <li>🔥 对 AIGC 保持高度热情，自学即梦 / Lovart / Agent 智能体搭建，持续关注行业前沿动态</li>
        <li>⚡ 执行力强，学习能力突出，能快速适应新工具和新需求，适合快节奏内容团队</li>
        <li>🤝 性格开朗，沟通积极，有团队协作经验，乐于探索新鲜事物</li>
      </ul>
    </div>
  </section>

  <!-- ===== 底部联系 ===== -->
  <footer class="footer">
    <div class="container">
      <p class="footer-title">联系我</p>
      <p class="footer-info">📧 2643105249@qq.com &nbsp;·&nbsp; 📱 18299125958</p>
      <p class="footer-copy">© 2026 盛欣格 / Sheng</p>
    </div>
  </footer>
```

- [ ] **步骤 2：在 `<style>` 中添加关于我和底部样式**

```css
    /* ===== 关于我 ===== */
    .about {
      background: var(--cream);
      padding: 30px 20px;
    }

    .about-list {
      list-style: none;
      padding: 0;
    }

    .about-list li {
      font-size: 13px;
      color: var(--dark);
      padding: 8px 0;
      border-bottom: 1px solid rgba(0,0,0,0.05);
      line-height: 1.6;
    }

    .about-list li:last-child {
      border-bottom: none;
    }

    /* ===== 底部联系 ===== */
    .footer {
      background: var(--dark);
      color: #fff;
      text-align: center;
      padding: 30px 20px;
    }

    .footer-title {
      font-size: 16px;
      font-weight: bold;
      margin-bottom: 8px;
    }

    .footer-info {
      font-size: 12px;
      color: #b2bec3;
      margin-bottom: 14px;
    }

    .footer-copy {
      font-size: 11px;
      color: var(--gray);
    }
```

- [ ] **步骤 3：浏览器刷新，确认底部深色背景、信息完整**

- [ ] **步骤 4：Git 提交**

```bash
git add index.html
git commit -m "feat: 添加关于我和底部联系板块"
```

---

### 任务 7：响应式适配

**文件：**
- 修改：`index.html`

- [ ] **步骤 1：在 `</style>` 前添加媒体查询**

```css
    /* ============================
       响应式适配
       ============================ */

    /* 平板端：内容居中，稍宽 */
    @media (min-width: 768px) {
      .container {
        padding: 0 40px;
      }

      .hero {
        padding: 70px 20px 50px;
      }

      .avatar {
        width: 120px;
        height: 120px;
      }

      .hero-name {
        font-size: 44px;
      }

      .work-grid {
        grid-template-columns: repeat(3, 1fr);
        gap: 12px;
      }

      .work-item img {
        height: 180px;
      }
    }

    /* 桌面端：更大间距 */
    @media (min-width: 1024px) {
      .container {
        padding: 0 60px;
      }

      .hero {
        padding: 80px 20px 60px;
      }

      .hero-name {
        font-size: 52px;
      }

      .skill-tags {
        gap: 10px;
      }

      .skill-tags span {
        font-size: 14px;
        padding: 8px 18px;
      }

      .work-grid {
        grid-template-columns: repeat(4, 1fr);
        gap: 14px;
      }

      .work-item img {
        height: 200px;
      }
    }
```

- [ ] **步骤 2：浏览器 + 手机预览测试**

用 Chrome DevTools 的 Device Toolbar（F12 → 手机图标），分别预览 iPhone SE、iPad、Desktop 三种尺寸，确认：  
- 文字不满屏溢出  
- 作品网格列数正确（手机3列 → 桌面4列）  
- Hero 区域上下间距合适

- [ ] **步骤 3：Git 提交**

```bash
git add index.html
git commit -m "feat: 添加响应式适配（手机/平板/桌面）"
```

---

### 任务 8：最终检查与提交

- [ ] **步骤 1：检查所有图片路径**

确认 `index.html` 中所有 `<img src="...">` 的文件名与项目目录中的实际文件名完全一致（注意中文文件名）。

- [ ] **步骤 2：完整浏览测试**

从顶部到尾部滚动浏览，检查：  
- 所有图片加载正常  
- 板块顺序正确（Hero → 技能 → 经历 → 作品 → 关于我 → 底部）  
- 彩虹条首尾呼应  
- 无文字重叠或样式错误

- [ ] **步骤 3：最终提交**

```bash
git add index.html
git commit -m "feat: 完成个人作品集网站搭建"
```

---

## 自审清单

| 项目 | 状态 |
|------|------|
| 覆盖所有设计文档要求 | ✅ 6大板块 + 响应式 + 别名 + 技能标签 |
| 无 TODO / 占位符 | ✅ 所有步骤均有完整代码 |
| 文件路径正确 | ✅ 引用已有图片文件名与目录一致 |
| 中文注释 | ✅ 所有注释使用中文 |
| 代码简单易懂 | ✅ 纯 HTML+CSS，无框架，无 JS |
