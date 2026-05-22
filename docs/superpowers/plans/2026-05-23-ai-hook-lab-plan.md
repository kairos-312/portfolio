# AI Hook Lab 实现计划

> **给执行者：** 使用 superpowers:executing-plans 或 superpowers:subagent-driven-development 来按任务逐步实现。步骤使用 `- [ ]` 复选框追踪进度。

**目标：** 创建一个单文件 HTML 网页应用，用户输入主题、选择平台和内容类型后，一键生成 10 个不同风格的爆款 Hook 文案。

**架构：** 单个 `index.html` 文件，包含所有 HTML/CSS/JS。使用模板引擎在本地生成 Hook，数据通过 localStorage 持久化收藏和历史记录。零依赖、零网络请求。

**技术栈：** 纯 HTML + CSS + 原生 JavaScript，无框架、无库。

---

## 文件结构

```
个人网页/
├── index.html          ← 创建：AI Hook Lab 完整应用
├── .gitignore          ← 修改：添加 .superpowers/
└── docs/superpowers/
    ├── specs/          ← 已创建：设计文档
    └── plans/          ← 已创建：本计划文件
```

---

### 任务 1：创建 HTML 骨架和 CSS 基础样式

**文件：**
- 创建：`index.html`

- [ ] **步骤 1：写入完整 HTML 结构骨架**

包含：head（meta viewport + 标题）、header（Logo + 副标题）、main（输入卡片 + Tab 栏 + 结果区 + 收藏区 + 历史区）、footer。CSS 使用暖白极简风配色。

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>AI Hook Lab - 爆款开头生成器</title>
<style>
  /* ===== 全局基础 ===== */
  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
  body {
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", "PingFang SC", "Hiragino Sans GB", "Microsoft YaHei", sans-serif;
    background: #fafaf9;
    color: #1a1a1a;
    line-height: 1.6;
    min-height: 100vh;
  }
  .container {
    max-width: 640px;
    margin: 0 auto;
    padding: 0 16px;
  }

  /* ===== 头部 ===== */
  .header {
    text-align: center;
    padding: 48px 0 32px;
  }
  .header .logo {
    font-size: 28px;
    font-weight: 800;
    color: #1a1a1a;
    letter-spacing: -0.5px;
  }
  .header .subtitle {
    font-size: 14px;
    color: #999;
    margin-top: 6px;
  }

  /* ===== 输入卡片 ===== */
  .input-card {
    background: #fff;
    border: 1px solid #e8e6e3;
    border-radius: 16px;
    padding: 24px;
    margin-bottom: 20px;
  }
  .input-card .label {
    font-size: 13px;
    font-weight: 600;
    color: #555;
    margin-bottom: 8px;
  }
  .input-card input[type="text"] {
    width: 100%;
    padding: 12px 16px;
    border: 1px solid #e8e6e3;
    border-radius: 10px;
    background: #fafaf9;
    font-size: 15px;
    color: #1a1a1a;
    outline: none;
    transition: border-color 0.2s;
  }
  .input-card input[type="text"]:focus {
    border-color: #f97316;
    background: #fff;
  }

  /* ===== 药丸选择按钮 ===== */
  .pill-group {
    display: flex;
    flex-wrap: wrap;
    gap: 8px;
    margin-bottom: 16px;
  }
  .pill {
    padding: 8px 16px;
    border-radius: 20px;
    border: 1px solid #e8e6e3;
    background: #f5f5f4;
    color: #888;
    font-size: 14px;
    cursor: pointer;
    transition: all 0.2s;
    user-select: none;
  }
  .pill:hover { background: #ededec; }
  .pill.active {
    background: #f97316;
    border-color: #f97316;
    color: #fff;
  }

  /* ===== 生成按钮 ===== */
  .btn-generate {
    width: 100%;
    padding: 14px;
    background: #1a1a1a;
    color: #fff;
    border: none;
    border-radius: 12px;
    font-size: 15px;
    font-weight: 600;
    cursor: pointer;
    transition: all 0.2s;
  }
  .btn-generate:hover { opacity: 0.9; transform: translateY(-1px); }
  .btn-generate:active { transform: translateY(0); }
  .btn-generate:disabled {
    opacity: 0.5;
    cursor: not-allowed;
    transform: none;
  }

  /* ===== Tab 栏 ===== */
  .tabs {
    display: flex;
    gap: 4px;
    border-bottom: 1px solid #e8e6e3;
    margin-bottom: 16px;
  }
  .tab {
    padding: 10px 18px;
    font-size: 14px;
    color: #999;
    cursor: pointer;
    border-bottom: 2px solid transparent;
    transition: all 0.2s;
    background: none;
    border-top: none;
    border-left: none;
    border-right: none;
  }
  .tab:hover { color: #555; }
  .tab.active {
    color: #1a1a1a;
    font-weight: 600;
    border-bottom-color: #f97316;
  }

  /* ===== Hook 卡片 ===== */
  .hook-card {
    background: #fff;
    border: 1px solid #e8e6e3;
    border-radius: 14px;
    padding: 18px;
    margin-bottom: 12px;
    transition: all 0.2s;
    border-left: 4px solid #f97316;
  }
  .hook-card:hover { box-shadow: 0 2px 12px rgba(0,0,0,0.06); }
  .hook-card .card-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 10px;
  }
  .hook-card .style-tag {
    padding: 3px 12px;
    border-radius: 12px;
    font-size: 12px;
    font-weight: 600;
  }
  .hook-card .score {
    font-weight: 800;
    font-size: 16px;
    color: #22c55e;
  }
  .hook-card .hook-text {
    font-size: 15px;
    font-weight: 600;
    color: #1a1a1a;
    line-height: 1.7;
    margin-bottom: 8px;
  }
  .hook-card .hook-reason {
    font-size: 13px;
    color: #888;
    line-height: 1.5;
  }
  .hook-card .card-actions {
    display: flex;
    gap: 10px;
    margin-top: 10px;
    justify-content: flex-end;
  }
  .hook-card .card-actions button {
    background: none;
    border: 1px solid #e8e6e3;
    border-radius: 8px;
    padding: 6px 12px;
    font-size: 13px;
    cursor: pointer;
    color: #888;
    transition: all 0.2s;
  }
  .hook-card .card-actions button:hover {
    background: #f5f5f4;
    color: #1a1a1a;
  }

  /* ===== 动画 ===== */
  @keyframes fadeInUp {
    from { opacity: 0; transform: translateY(12px); }
    to { opacity: 1; transform: translateY(0); }
  }
  .hook-card { animation: fadeInUp 0.4s ease forwards; }
  .hook-card:nth-child(2) { animation-delay: 0.05s; }
  .hook-card:nth-child(3) { animation-delay: 0.1s; }
  .hook-card:nth-child(4) { animation-delay: 0.15s; }
  .hook-card:nth-child(5) { animation-delay: 0.2s; }
  .hook-card:nth-child(6) { animation-delay: 0.25s; }
  .hook-card:nth-child(7) { animation-delay: 0.3s; }
  .hook-card:nth-child(8) { animation-delay: 0.35s; }
  .hook-card:nth-child(9) { animation-delay: 0.4s; }
  .hook-card:nth-child(10) { animation-delay: 0.45s; }

  @keyframes shimmer {
    0% { background-position: -200px 0; }
    100% { background-position: 200px 0; }
  }
  .skeleton {
    background: linear-gradient(90deg, #f0f0f0 25%, #e0e0e0 50%, #f0f0f0 75%);
    background-size: 400px 100%;
    animation: shimmer 1.5s ease-in-out infinite;
    border-radius: 14px;
    height: 100px;
    margin-bottom: 12px;
  }

  /* ===== 空状态 ===== */
  .empty-state {
    text-align: center;
    padding: 48px 20px;
    color: #ccc;
  }
  .empty-state .empty-icon { font-size: 48px; margin-bottom: 12px; }
  .empty-state p { font-size: 14px; margin-bottom: 16px; }
  .empty-state .quick-topics {
    display: flex;
    flex-wrap: wrap;
    gap: 8px;
    justify-content: center;
  }
  .empty-state .quick-topic {
    padding: 6px 14px;
    background: #f5f5f4;
    border: 1px solid #e8e6e3;
    border-radius: 20px;
    font-size: 13px;
    color: #888;
    cursor: pointer;
    transition: all 0.2s;
  }
  .empty-state .quick-topic:hover {
    background: #fff;
    color: #f97316;
    border-color: #f97316;
  }

  /* ===== 历史列表项 ===== */
  .history-item {
    background: #fff;
    border: 1px solid #e8e6e3;
    border-radius: 12px;
    padding: 14px;
    margin-bottom: 8px;
    cursor: pointer;
    transition: all 0.2s;
  }
  .history-item:hover { border-color: #f97316; }
  .history-item .history-meta {
    font-size: 12px;
    color: #999;
    margin-top: 4px;
  }

  /* ===== Footer ===== */
  .footer {
    text-align: center;
    padding: 32px 0;
    color: #ccc;
    font-size: 12px;
    border-top: 1px solid #e8e6e3;
    margin-top: 32px;
  }

  /* ===== 响应式 ===== */
  @media (max-width: 480px) {
    .header { padding: 32px 0 20px; }
    .header .logo { font-size: 24px; }
    .input-card { padding: 16px; }
    .pill { padding: 6px 12px; font-size: 13px; }
    .hook-card { padding: 14px; }
    .hook-card .hook-text { font-size: 14px; }
  }
</style>
</head>
<body>
<div class="container">
  <header class="header">
    <div class="logo">⚡ AI Hook Lab</div>
    <p class="subtitle">一键生成10个爆款开头，让你的内容赢在起跑线</p>
  </header>

  <main>
    <!-- 输入区 -->
    <div class="input-card">
      <div class="label" style="margin-bottom: 8px;">🎯 输入主题</div>
      <input type="text" id="topicInput" placeholder="例如：夏日护肤、AI赚钱、考研经验...">
      <div style="margin-top: 16px;">
        <div class="label" style="margin-bottom: 8px;">📱 选择平台</div>
        <div class="pill-group" id="platformGroup">
          <span class="pill active" data-value="xiaohongshu">小红书</span>
          <span class="pill" data-value="douyin">抖音</span>
          <span class="pill" data-value="bilibili">B站</span>
        </div>
      </div>
      <div>
        <div class="label" style="margin-bottom: 8px;">📹 内容类型</div>
        <div class="pill-group" id="typeGroup">
          <span class="pill active" data-value="video">视频</span>
          <span class="pill" data-value="image-text">图文</span>
          <span class="pill" data-value="product">产品广告</span>
          <span class="pill" data-value="tutorial">教程</span>
          <span class="pill" data-value="opinion">观点帖</span>
        </div>
      </div>
      <button class="btn-generate" id="btnGenerate">✨ 生成10个爆款Hook</button>
    </div>

    <!-- Tab 栏 -->
    <div class="tabs">
      <button class="tab active" data-tab="results">生成结果</button>
      <button class="tab" data-tab="favorites">收藏夹 <span id="favCount" style="color:#ccc;">0</span></button>
      <button class="tab" data-tab="history">历史记录</button>
    </div>

    <!-- 结果区 -->
    <div id="resultsPanel"></div>
    <!-- 收藏区（默认隐藏） -->
    <div id="favoritesPanel" style="display:none;"></div>
    <!-- 历史区（默认隐藏） -->
    <div id="historyPanel" style="display:none;"></div>
  </main>

  <footer class="footer">
    AI Hook Lab · 数据保存在本地浏览器
  </footer>
</div>
```

- [ ] **步骤 2：验证 HTML 骨架在浏览器中正常显示**

在浏览器打开 `index.html`，检查：header 显示、输入卡片布局、药丸按钮可点击切换 `.active`、Tab 栏显示、空状态提示。

- [ ] **步骤 3：提交**

```bash
git add index.html .gitignore docs/
git commit -m "feat: 创建 AI Hook Lab HTML 骨架和基础 CSS 样式"
```

---

### 任务 2：实现 Hook 模板引擎（JavaScript 数据层）

**文件：**
- 修改：`index.html`（在 `</body>` 前添加 `<script>` 块）

- [ ] **步骤 1：添加模板数据和生成函数**

```javascript
<script>
// ===== 数据：Hook 风格模板库 =====
const STYLES = [
  {
    id: 'controversy',
    name: '争议型',
    icon: '🔥',
    color: '#f97316',
    bg: '#fff7ed',
    templates: [
      '我用了{time}才明白，99%的人根本不懂{topic}！',
      '为什么你做了{effort}，{topic}还是没效果？因为大多数人都搞错了',
      '别再做{counter}了，这才是{topic}的正确打开方式',
      '{topic}圈最大的骗局，你肯定中过招',
      '关于{topic}，全网都在传播一个错误认知',
      '专家不会告诉你的{topic}真相，第3个太扎心了',
    ]
  },
  {
    id: 'data',
    name: '数据型',
    icon: '📊',
    color: '#3b82f6',
    bg: '#eff6ff',
    templates: [
      '测试了{num}{unit}{topic}，只有这{small}款真的有用',
      '花了{budget}实测{topic}，结果让我崩溃了',
      '{topic}能让你多赚{amount}？我用数据告诉你答案',
      '研究了{num}个爆款账号，发现{topic}都用了这{small}招',
      '连续{num}天做{topic}，我记录了这组惊人的数据',
      '调研了{num}位用户，关于{topic}，{percent}%的人选了这个',
    ]
  },
  {
    id: 'suspense',
    name: '悬念型',
    icon: '😱',
    color: '#8b5cf6',
    bg: '#f5f3ff',
    templates: [
      '{expert}打死也不用的{num}种{topic}，第{rank}种你家就有',
      '做了{num}年{topic}，今天才发现的惊天秘密',
      '如果你只知道{topic}的这{num}个真相，你会崩溃的',
      '关于{topic}，有一个真相没人敢告诉你',
      '我发现了{topic}的一个致命漏洞，看完你就懂了',
      '这个{topic}的秘密，我犹豫了{num}年才说出来',
    ]
  },
  {
    id: 'value',
    name: '干货型',
    icon: '💡',
    color: '#22c55e',
    bg: '#f0fdf4',
    templates: [
      '{topic}全攻略：从入门到精通，看这一篇就够了',
      '新手做{topic}，记住这{num}个要点少走{num}年弯路',
      '{topic}保姆级教程，{num}分钟教会你',
      '关于{topic}，我把所有经验浓缩成这{num}条精华',
      '搞懂{topic}，只需掌握这{num}个核心方法',
      '吐血整理！{topic}全流程拆解，建议收藏',
    ]
  },
  {
    id: 'empathy',
    name: '共鸣型',
    icon: '😂',
    color: '#ec4899',
    bg: '#fdf2f8',
    templates: [
      '做{topic}的人，都经历过这{num}个崩溃瞬间',
      '如果你也在为{topic}焦虑，请花{num}分钟看完',
      '每一个{topic}失败的人，都卡在了这{num}步',
      '{topic}之前我以为很简单，直到……',
      '致所有正在{topic}的你：你并不孤单',
      '当你说{topic}很累时，我来告诉你真实的经历',
    ]
  },
  {
    id: 'counter-intuitive',
    name: '反常识型',
    icon: '🎯',
    color: '#ef4444',
    bg: '#fef2f2',
    templates: [
      '越{topic}越容易失败？颠覆你认知的{num}个事实',
      '{topic}做得越多，效果越差？真相让人意外',
      '大家都在做{topic}，但真正有效的方法恰恰相反',
      '关于{topic}，科学证明我们一直搞反了',
      '停止做{topic}！你的努力可能正在害了你',
      '那些教你{topic}的人，可能自己都不会',
    ]
  },
  {
    id: 'story',
    name: '故事型',
    icon: '📖',
    color: '#06b6d4',
    bg: '#ecfeff',
    templates: [
      '从{start}到{end}，我做{topic}的{num}年经历全分享',
      '一个{topic}素人，如何用{num}个月逆袭？',
      '辞职做{topic}的第{num}天，我想说这些……',
      '那个被所有人嘲笑的{topic}项目，后来怎么样了？',
      '我拿了{budget}做{topic}，结果出乎所有人意料',
      '从一个{topic}小白到月入{amount}，我的真实复盘',
    ]
  },
  {
    id: 'urgency',
    name: '紧迫型',
    icon: '⚡',
    color: '#eab308',
    bg: '#fefce8',
    templates: [
      '别再观望了！{topic}的红利期只剩最后{num}个月',
      '错过{danger}你还能忍，错过{topic}你会后悔{num}年',
      '现在不做{topic}，{num}个月后你会后悔的',
      '所有人都在悄悄布局{topic}，你还不知道？',
      '警告：再不开始{topic}，你可能要错过整个{field}风口',
      '{time}年不做{topic}，你将彻底被淘汰',
    ]
  },
];

// ===== 平台语气调整词库 =====
const PLATFORM_TONE = {
  xiaohongshu: {
    time: ['一年', '三年', '两年', '半年'],
    effort: ['那么多功课', '无数努力', '大量时间', '那么多钱'],
    counter: ['盲目跟风', '照搬博主', '无效种草'],
    expert: ['皮肤科医生', '资深博主', '业内大咖', '头部博主'],
    start: ['零基础小白', '什么都不懂', '一脸懵'],
    end: ['月入过万', '粉丝10万', '成为博主', '广告接到手软'],
    danger: ['爆款', '涨粉机会', '流量红利'],
    field: ['小红书', '内容赛道', '流量'],
    amount: ['1万', '5万', '10万'],
    budget: ['5000块', '1万块', '3万块'],
    num: ['10', '20', '30', '47', '100'],
    small: ['3', '5', '2'],
    rank: ['3', '2', '1'],
    unit: ['个', '款', '种'],
    percent: ['87', '93', '76', '95'],
  },
  douyin: {
    time: ['两年', '三年', '一年', '五年'],
    effort: ['那么多精力', '无数个通宵', '疯狂投入'],
    counter: ['瞎跟风', '乱模仿', '盲目追热点'],
    expert: ['千万网红', 'MCN老板', '流量操盘手', '算法工程师'],
    start: ['0粉丝', '啥也不会', '一个小白'],
    end: ['百万粉丝', '日入过万', '条条爆款'],
    danger: ['流量红利', '涨粉风口', '热门赛道'],
    field: ['抖音', '短视频', '直播带货'],
    amount: ['5万', '10万', '50万'],
    budget: ['1万块', '5万块', '10万块'],
    num: ['50', '100', '200', '500', '1000'],
    small: ['3', '5', '2'],
    rank: ['2', '3', '1'],
    unit: ['条', '个', '种'],
    percent: ['90', '85', '78', '92'],
  },
  bilibili: {
    time: ['三年', '五年', '两年', '四年'],
    effort: ['那么多努力', '无数个深夜', '反复尝试'],
    counter: ['跟风UP主', '照搬教程', '盲目模仿'],
    expert: ['百大UP主', '行业大佬', '技术专家', '资深UP'],
    start: ['完全不懂', '编程小白', '门外汉'],
    end: ['成为UP主', '涨粉百万', '接到商单'],
    danger: ['创作激励', '粉丝增长期', '内容蓝海'],
    field: ['B站', '内容创作', '知识区'],
    amount: ['3万', '8万', '20万'],
    budget: ['3000块', '8000块', '2万块'],
    num: ['20', '50', '100', '200', '365'],
    small: ['3', '5', '2'],
    rank: ['3', '2', '1'],
    unit: ['个', '种', '类'],
    percent: ['82', '91', '74', '88'],
  },
};

// ===== 内容类型对模板的选择权重 =====
// 每种平台×内容类型，对应最适合的模板风格组合
const TYPE_STYLE_MAP = {
  video: ['controversy', 'suspense', 'story', 'urgency', 'empathy', 'data', 'counter-intuitive', 'value'],
  'image-text': ['data', 'value', 'empathy', 'controversy', 'suspense', 'counter-intuitive', 'story', 'urgency'],
  product: ['data', 'urgency', 'value', 'controversy', 'counter-intuitive', 'suspense', 'story', 'empathy'],
  tutorial: ['value', 'data', 'story', 'suspense', 'counter-intuitive', 'controversy', 'empathy', 'urgency'],
  opinion: ['controversy', 'counter-intuitive', 'suspense', 'empathy', 'story', 'data', 'value', 'urgency'],
};

// ===== 推荐理由库（按风格） =====
const REASONS = {
  controversy: [
    '认知反差制造争议感，适合{topic}类内容，快速筛选目标受众并引发讨论',
    '挑战常识的说法激发好奇心，用户评论意愿强，互动率通常高于普通开头',
  ],
  data: [
    '具体数字增加可信度，"{num}"这样的精确数据制造专业感，适合测评/合集类内容',
    '数据锚定效应让用户对内容产生信任，点击率比模糊描述高30%以上',
  ],
  suspense: [
    '"{expert}打死也不"类句式制造强悬念，结合熟悉场景拉近距离，促使用户完播',
    '制造"信息缺口"，用户大脑会自动渴望填补，是最有效的完播钩子之一',
  ],
  value: [
    '明确承诺实用价值，"从入门到精通"降低用户学习门槛感，适合教程/攻略类内容',
    '干货清单式开头给用户确定预期，收藏率远高于其他类型',
  ],
  empathy: [
    '用"我们都经历过"的共鸣感降低用户防御心理，适合情感/经历分享类内容',
    '情绪共鸣是最快的连接方式，用户会因为"是我是我"的感觉而停留',
  ],
  'counter-intuitive': [
    '打破常识的信息天然具有传播力，"相反"类句式制造惊讶，适合观点输出',
    '反常识信息触发大脑的"纠错机制"，用户会不由自主想验证真假',
  ],
  story: [
    '微型叙事自带悬念和代入感，"{start}到{end}"制造弧线，用户想看到结局',
    '故事是人类最原始的注意力捕获器，比任何技巧都更自然有效',
  ],
  urgency: [
    '时间压力促使用户立即行动，"只有最后{num}个月"制造稀缺感，适合产品推广',
    'FOMO（错失恐惧）心理触发即时的点击欲望，是转化率最高的开头类型之一',
  ],
};

// ===== 辅助函数 =====
function randomPick(arr) {
  return arr[Math.floor(Math.random() * arr.length)];
}

function shuffle(arr) {
  const a = [...arr];
  for (let i = a.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1));
    [a[i], a[j]] = [a[j], a[i]];
  }
  return a;
}

// ===== 核心生成函数 =====
function generateHooks(topic, platform, contentType) {
  const tone = PLATFORM_TONE[platform];
  const styleOrder = TYPE_STYLE_MAP[contentType];
  const hooks = [];

  styleOrder.forEach((styleId, index) => {
    const style = STYLES.find(s => s.id === styleId);
    let template = randomPick(style.templates);

    // 填充模板变量
    const vars = {
      topic: topic,
      time: randomPick(tone.time),
      effort: randomPick(tone.effort),
      counter: randomPick(tone.counter),
      expert: randomPick(tone.expert),
      start: randomPick(tone.start),
      end: randomPick(tone.end),
      danger: randomPick(tone.danger),
      field: randomPick(tone.field),
      amount: randomPick(tone.amount),
      budget: randomPick(tone.budget),
      num: randomPick(tone.num),
      small: randomPick(tone.small),
      rank: randomPick(tone.rank),
      unit: randomPick(tone.unit),
      percent: randomPick(tone.percent),
    };

    const hookText = template.replace(/\{(\w+)\}/g, (_, key) => vars[key] || `{${key}}`);

    // 点击率评分（基础分 + 风格权重 + 随机微调）
    const baseScore = 80;
    const styleBonus = [0, 2, 4, 6, 8, 10, 12, 14][index]; // 排前面的风格加分
    const randomNoise = Math.floor(Math.random() * 6) - 2; // -2~+3
    const score = Math.min(98, Math.max(78, baseScore + styleBonus + randomNoise));

    // 推荐理由
    const reasonTemplate = randomPick(REASONS[styleId]);
    const reason = reasonTemplate.replace(/\{(\w+)\}/g, (_, key) => vars[key] || `{${key}}`);

    hooks.push({
      id: Date.now() + '_' + index,
      hook: hookText,
      style: style.name,
      styleIcon: style.icon,
      styleColor: style.color,
      styleBg: style.bg,
      score: score,
      reason: reason,
    });
  });

  return hooks;
}
</script>
```

- [ ] **步骤 2：在浏览器控制台测试生成函数**

打开浏览器控制台，运行：
```javascript
const result = generateHooks('夏日护肤', 'xiaohongshu', 'video');
console.log(result);
```
验证输出 10 个 Hook，每个包含 hook、style、score、reason 字段。

- [ ] **步骤 3：提交**

```bash
git add index.html
git commit -m "feat: 实现 Hook 模板引擎和生成函数"
```

---

### 任务 3：实现交互逻辑（UI 层）

**文件：**
- 修改：`index.html`（在模板引擎脚本后继续添加交互代码）

- [ ] **步骤 1：添加完整交互逻辑**

```javascript
// ===== DOM 元素 =====
const topicInput = document.getElementById('topicInput');
const btnGenerate = document.getElementById('btnGenerate');
const resultsPanel = document.getElementById('resultsPanel');
const favoritesPanel = document.getElementById('favoritesPanel');
const historyPanel = document.getElementById('historyPanel');
const favCountEl = document.getElementById('favCount');

// ===== 状态 =====
let currentPlatform = 'xiaohongshu';
let currentType = 'video';
let currentTab = 'results';
let currentHooks = [];
let favorites = JSON.parse(localStorage.getItem('hooklab_favorites') || '[]');
let history = JSON.parse(localStorage.getItem('hooklab_history') || '[]');

// ===== 平台选择 =====
document.getElementById('platformGroup').addEventListener('click', (e) => {
  if (e.target.classList.contains('pill')) {
    e.target.parentElement.querySelectorAll('.pill').forEach(p => p.classList.remove('active'));
    e.target.classList.add('active');
    currentPlatform = e.target.dataset.value;
  }
});

// ===== 内容类型选择 =====
document.getElementById('typeGroup').addEventListener('click', (e) => {
  if (e.target.classList.contains('pill')) {
    e.target.parentElement.querySelectorAll('.pill').forEach(p => p.classList.remove('active'));
    e.target.classList.add('active');
    currentType = e.target.dataset.value;
  }
});

// ===== Tab 切换 =====
document.querySelector('.tabs').addEventListener('click', (e) => {
  if (e.target.classList.contains('tab')) {
    document.querySelectorAll('.tab').forEach(t => t.classList.remove('active'));
    e.target.classList.add('active');
    currentTab = e.target.dataset.tab;

    resultsPanel.style.display = currentTab === 'results' ? 'block' : 'none';
    favoritesPanel.style.display = currentTab === 'favorites' ? 'block' : 'none';
    historyPanel.style.display = currentTab === 'history' ? 'block' : 'none';

    if (currentTab === 'favorites') renderFavorites();
    if (currentTab === 'history') renderHistory();
  }
});

// ===== 空状态渲染 =====
function renderEmptyState() {
  resultsPanel.innerHTML = `
    <div class="empty-state">
      <div class="empty-icon">💡</div>
      <p>输入主题，选择平台和类型，一键生成爆款开头</p>
      <div class="quick-topics">
        <span class="quick-topic" onclick="quickFill('夏日护肤')">夏日护肤</span>
        <span class="quick-topic" onclick="quickFill('AI赚钱')">AI赚钱</span>
        <span class="quick-topic" onclick="quickFill('考研经验')">考研经验</span>
        <span class="quick-topic" onclick="quickFill('租房避坑')">租房避坑</span>
        <span class="quick-topic" onclick="quickFill('效率提升')">效率提升</span>
      </div>
    </div>
  `;
}

function quickFill(topic) {
  topicInput.value = topic;
  topicInput.focus();
  // 滚动到输入区
  document.querySelector('.input-card').scrollIntoView({ behavior: 'smooth' });
}

// ===== 骨架屏 =====
function renderSkeletons() {
  let html = '';
  for (let i = 0; i < 10; i++) {
    html += '<div class="skeleton"></div>';
  }
  resultsPanel.innerHTML = html;
}

// ===== Hook卡片渲染 =====
function renderHooks(hooks) {
  let html = '';
  hooks.forEach((h, i) => {
    const isFaved = favorites.some(f => f.id === h.id);
    html += `
      <div class="hook-card" style="border-left-color: ${h.styleColor}; animation-delay: ${i * 0.05}s;">
        <div class="card-header">
          <span class="style-tag" style="background: ${h.styleBg}; color: ${h.styleColor};">${h.styleIcon} ${h.style}</span>
          <div style="display:flex;align-items:center;gap:10px;">
            <span class="score">${h.score}%</span>
            <button onclick="copyHook('${escapeHtml(h.hook)}', this)" title="复制" style="background:none;border:none;cursor:pointer;font-size:16px;">📋</button>
            <button onclick="toggleFav('${h.id}', this)" title="${isFaved ? '取消收藏' : '收藏'}" style="background:none;border:none;cursor:pointer;font-size:16px;">${isFaved ? '❤️' : '🤍'}</button>
          </div>
        </div>
        <div class="hook-text">"${escapeHtml(h.hook)}"</div>
        <div class="hook-reason">💡 ${h.reason}</div>
      </div>
    `;
  });
  resultsPanel.innerHTML = html;
  currentHooks = hooks;
}

function escapeHtml(str) {
  const div = document.createElement('div');
  div.textContent = str;
  return div.innerHTML;
}

// ===== 复制功能 =====
function copyHook(hookText, btn) {
  navigator.clipboard.writeText(hookText).then(() => {
    btn.textContent = '✅';
    setTimeout(() => { btn.textContent = '📋'; }, 1500);
  });
}

// ===== 收藏功能 =====
function toggleFav(hookId, btn) {
  const hook = currentHooks.find(h => h.id === hookId);
  if (!hook) {
    // 可能是在收藏页操作，从 favorites 中查找
    const favHook = favorites.find(h => h.id === hookId);
    if (favHook) {
      favorites = favorites.filter(h => h.id !== hookId);
      btn.textContent = '🤍';
    }
    saveFavorites();
    renderFavorites();
    return;
  }

  const index = favorites.findIndex(h => h.id === hookId);
  if (index > -1) {
    favorites.splice(index, 1);
    btn.textContent = '🤍';
  } else {
    favorites.push({ ...hook, savedAt: Date.now() });
    btn.textContent = '❤️';
  }
  saveFavorites();
  updateFavCount();
}

function saveFavorites() {
  localStorage.setItem('hooklab_favorites', JSON.stringify(favorites));
}

function updateFavCount() {
  favCountEl.textContent = favorites.length > 0 ? favorites.length : '0';
  if (favorites.length > 0) {
    favCountEl.style.color = '#f97316';
  } else {
    favCountEl.style.color = '#ccc';
  }
}

function renderFavorites() {
  if (favorites.length === 0) {
    favoritesPanel.innerHTML = `
      <div class="empty-state">
        <div class="empty-icon">🤍</div>
        <p>还没有收藏哦，去生成结果页点击 🤍 收藏吧</p>
      </div>
    `;
    return;
  }
  // 复用 renderHooks 但渲染到 favoritesPanel
  currentHooks = favorites;
  let html = '';
  favorites.forEach((h, i) => {
    html += `
      <div class="hook-card" style="border-left-color: ${h.styleColor};">
        <div class="card-header">
          <span class="style-tag" style="background: ${h.styleBg}; color: ${h.styleColor};">${h.styleIcon} ${h.style}</span>
          <div style="display:flex;align-items:center;gap:10px;">
            <span class="score">${h.score}%</span>
            <button onclick="copyHook('${escapeHtml(h.hook)}', this)" style="background:none;border:none;cursor:pointer;font-size:16px;">📋</button>
            <button onclick="toggleFav('${h.id}', this)" style="background:none;border:none;cursor:pointer;font-size:16px;">❤️</button>
          </div>
        </div>
        <div class="hook-text">"${escapeHtml(h.hook)}"</div>
        <div class="hook-reason">💡 ${h.reason}</div>
      </div>
    `;
  });
  favoritesPanel.innerHTML = html;
}

// ===== 历史记录功能 =====
function saveHistory(topic, platform, contentType, hooks) {
  const record = {
    id: Date.now(),
    topic,
    platform,
    contentType,
    hooks: hooks.map(h => ({ ...h })), // 浅拷贝
    createdAt: new Date().toLocaleString('zh-CN'),
  };
  history.unshift(record);
  if (history.length > 20) history = history.slice(0, 20);
  localStorage.setItem('hooklab_history', JSON.stringify(history));
}

function renderHistory() {
  if (history.length === 0) {
    historyPanel.innerHTML = `
      <div class="empty-state">
        <div class="empty-icon">📜</div>
        <p>还没有生成记录，输入主题开始吧</p>
      </div>
    `;
    return;
  }
  let html = '';
  history.forEach((record) => {
    const platformNames = { xiaohongshu: '小红书', douyin: '抖音', bilibili: 'B站' };
    const typeNames = { video: '视频', 'image-text': '图文', product: '产品广告', tutorial: '教程', opinion: '观点帖' };
    html += `
      <div class="history-item" onclick="loadHistory('${record.id}')">
        <div style="font-weight:600;color:#1a1a1a;">${escapeHtml(record.topic)}</div>
        <div class="history-meta">
          ${platformNames[record.platform]} · ${typeNames[record.contentType]} · ${record.hooks.length}个Hook · ${record.createdAt}
        </div>
      </div>
    `;
  });
  historyPanel.innerHTML = html;
}

function loadHistory(recordId) {
  const record = history.find(r => r.id === Number(recordId));
  if (!record) return;
  currentPlatform = record.platform;
  currentType = record.contentType;
  topicInput.value = record.topic;
  
  // 更新平台药丸
  document.querySelectorAll('#platformGroup .pill').forEach(p => {
    p.classList.toggle('active', p.dataset.value === record.platform);
  });
  // 更新类型药丸
  document.querySelectorAll('#typeGroup .pill').forEach(p => {
    p.classList.toggle('active', p.dataset.value === record.contentType);
  });
  
  // 切换到结果 tab
  document.querySelectorAll('.tab').forEach(t => t.classList.remove('active'));
  document.querySelector('[data-tab="results"]').classList.add('active');
  currentTab = 'results';
  resultsPanel.style.display = 'block';
  favoritesPanel.style.display = 'none';
  historyPanel.style.display = 'none';
  
  renderHooks(record.hooks);
  resultsPanel.scrollIntoView({ behavior: 'smooth' });
}

// ===== 生成按钮点击 =====
btnGenerate.addEventListener('click', () => {
  const topic = topicInput.value.trim();
  if (!topic) {
    topicInput.focus();
    topicInput.style.borderColor = '#ef4444';
    setTimeout(() => { topicInput.style.borderColor = '#e8e6e3'; }, 2000);
    return;
  }

  // 显示骨架屏
  btnGenerate.disabled = true;
  btnGenerate.textContent = '⏳ 正在生成...';
  
  // 切换到结果 tab
  document.querySelectorAll('.tab').forEach(t => t.classList.remove('active'));
  document.querySelector('[data-tab="results"]').classList.add('active');
  currentTab = 'results';
  resultsPanel.style.display = 'block';
  favoritesPanel.style.display = 'none';
  historyPanel.style.display = 'none';
  
  renderSkeletons();
  resultsPanel.scrollIntoView({ behavior: 'smooth' });

  // 模拟 AI 思考延迟 1.2~1.8 秒
  const delay = 1200 + Math.random() * 600;
  setTimeout(() => {
    const hooks = generateHooks(topic, currentPlatform, currentType);
    renderHooks(hooks);
    saveHistory(topic, currentPlatform, currentType, hooks);
    
    btnGenerate.disabled = false;
    btnGenerate.textContent = '✨ 生成10个爆款Hook';
  }, delay);
});

// ===== 支持回车键生成 =====
topicInput.addEventListener('keydown', (e) => {
  if (e.key === 'Enter') {
    btnGenerate.click();
  }
});

// ===== 初始化 =====
renderEmptyState();
updateFavCount();
```

- [ ] **步骤 2：验证完整交互流程**

在浏览器中测试：
1. 输入主题"夏日护肤" → 选平台"小红书" → 选类型"视频" → 点击生成
2. 确认显示骨架屏 → 1秒多后显示10张Hook卡片（带渐入动画）
3. 点击某张卡片的 📋 → 确认变 ✅ → 1.5秒恢复
4. 点击某张卡片的 🤍 → 确认变 ❤️ → 切换到收藏夹Tab → 确认显示
5. 切回历史记录Tab → 确认有记录 → 点击记录 → 确认重新加载
6. 用手机尺寸测试响应式布局

- [ ] **步骤 3：提交**

```bash
git add index.html
git commit -m "feat: 实现完整交互逻辑（生成/复制/收藏/历史）"
```

---

### 任务 4：最终打磨和验证

**文件：**
- 修改：`index.html`（细节优化）

- [ ] **步骤 1：添加细节优化**

包括：
- 生成按钮的 hover 和 active 动效优化
- 卡片 hover 时轻微上浮效果
- 历史记录显示平台和类型的中文名称
- 移动端药丸按钮字号和间距微调

```css
/* 在现有 CSS 后追加 */

/* 卡片 hover 上浮 */
.hook-card {
  transition: all 0.2s ease;
}
.hook-card:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 16px rgba(0,0,0,0.08);
}

/* 历史项 hover */
.history-item:hover {
  transform: translateY(-1px);
  box-shadow: 0 2px 8px rgba(0,0,0,0.04);
}

/* 移动端微调 */
@media (max-width: 480px) {
  .pill-group { gap: 6px; }
  .pill { padding: 5px 10px; font-size: 12px; }
  .hook-card .card-header { flex-direction: column; align-items: flex-start; gap: 8px; }
  .hook-card .card-header > div:last-child { align-self: flex-end; }
}
```

- [ ] **步骤 2：完整走查一遍所有功能**

测试清单：
- [ ] 空状态显示快速填入标签，点击填入主题
- [ ] 输入为空时点击生成 → 输入框变红提示
- [ ] 选不同平台生成 → 验证语气有差异
- [ ] 选不同内容类型生成 → 验证风格顺序有差异
- [ ] 收藏→切Tab查看→取消收藏
- [ ] 历史→点击加载→确认恢复
- [ ] 关闭再打开 → 收藏和历史仍然在
- [ ] 桌面 640px、平板 480px、手机 375px 都正常
- [ ] 回车键可以触发生成

- [ ] **步骤 3：最终提交**

```bash
git add index.html
git commit -m "feat: 最终打磨 — hover 动效、响应式微调、交互细节优化"
```
