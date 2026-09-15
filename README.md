# NOAH Content Radar · 诺亚全平台选题中枢

> 服务于诺亚品牌部全平台内容策划团队的每日选题台
> 每天早上 8:00 自动刷新 · 4 大模块 · 9 个平台一键复制 Prompt + 1 个CIO专属通道

---

## 🎯 产品定位

诺亚选题雷达是面向诺亚全平台媒体矩阵的**每日选题中枢**——不再是 YouTube 单平台工具，而是覆盖官网、小红书、视频号、Facebook、LinkedIn、Medium、YouTube、Reddit、Quora 的全平台内容引擎，另有 X 作为CIO日报专属输出口独立运行。

**四大模块**：
- 🌅 今日早报 · 3 条最火话题 + 评分 + 适合平台
- 📰 重点新闻 · 宏观 / 市场 / 高净值华人三 Tab
- 📅 90 天日历 · 全球财经硬节点（纯财经，去营销节点）
- 🎬 全平台选题生成器 · 每个选题下挂 9 个平台 Prompt 按钮

**核心交互**：每个选题卡片下方有 9 个平台按钮。点击任意按钮，复制该平台的专属 Prompt（含日报 Prompt v2.0 差异化铁律），到 Claude 对话粘贴即可生成对应平台的内容。X 不在这 9 个按钮之内，是CIO办公室日报的专属发布口，由Doris自行判断是否发布。

---

## 📁 项目结构

```
noah-radar-project/
├── index.html                  # 入口 HTML
├── package.json                # 依赖声明
├── vite.config.js              # Vite 配置
├── tailwind.config.js          # Tailwind 配置
├── postcss.config.js           # PostCSS 配置
└── src/
    ├── main.jsx                # React 入口
    ├── App.jsx                 # 主组件（四大模块）
    ├── index.css                # 全局样式
    ├── prompts.js               # 所有按钮的 Prompt 模板 ⭐
    └── data/
        └── data.json            # 所有动态数据（每日更新这个文件）⭐
```

**重点关注两个文件**：
- `src/data/data.json` —— 每天更新这个文件，网页就会有新内容
- `src/prompts.js` —— 9 个平台 Prompt + 每日抓取 Prompt 统一管理

> ⚠️ **本次README更新只覆盖文字与设计原则说明。`src/prompts.js` 里对应旧平台（微信公众号/知乎/X）的函数名和实际 Prompt 内容尚未同步修改，需要按下方新清单手工更新代码，否则按钮文字和实际生成的内容会对不上。**

---

## 🚀 第一次部署（10 分钟）

### 步骤 1：本地试跑

```bash
cd noah-radar-project
npm install       # 安装依赖（只需第一次）
npm run dev       # 启动本地开发服务器
```

浏览器会自动打开 http://localhost:5173，先在本地确认页面正常。

### 步骤 2：推到 GitHub

1. 到 [github.com/new](https://github.cm/new) 新建一个仓库，名字 `noah-radar`
2. **设为 Private**（含内容策略，不要公开）
3. 按 GitHub 给出的命令推送：

```bash
cd noah-radar-project
git init
git add .
git commit -m "init: 诺亚选题雷达 v2.1"
git branch -M main
git remote add origin https://github.com/你的账号/noah-radar.git
git push -u origin main
```

### 步骤 3：部署到 Cloudflare Pages

1. 打开 [pages.cloudflare.com](https://pages.cloudflare.com)，用 GitHub 账号登录
2. 点 "Create a project" → "Connect to Git" → 选 `noah-radar` 仓库
3. 构建配置：
   - **Framework preset**：`Vite`
   - **Build command**：`npm run build`
   - **Build output directory**：`dist`
   - 其他默认
4. 点 "Save and Deploy"，等 1-2 分钟
5. 完成后拿到链接：`https://noah-radar.pages.dev`

---

## 🔐 给链接加密码保护（建议做）

用 **Cloudflare Access**（免费）：
1. Cloudflare Dashboard → Zero Trust → Access → Applications
2. 添加 Self-hosted Application，指向你的 pages.dev 域名
3. 设置访问策略：只允许指定邮箱登录
4. 完全免费，50 人以内免费使用

---

## 🔄 每日 8:00 更新机制（Claude 定时任务方案）

### 一次性配置

1. 在 [Claude.ai](https://claude.ai) 网页端登录账号
2. 创建一个新对话，命名为「诺亚选题雷达 · 每日 8 点」
3. 在该对话中开启 **Scheduled Tasks（定时任务）** 功能
4. 设置：
   - 触发时间：每天 08:00（你的本地时区）
   - 任务内容：粘贴 `src/prompts.js` 中的 `DAILY_CRAWL_PROMPT`，把 `{YYYY-MM-DD}` 替换为 `{{today}}`
   - 保存

### 品牌部每日操作（约 2 分钟）

```
08:00  Claude 自动跑出新版 data.json（在定时任务对话里）
08:15  品牌部负责人收到通知 → 打开对话 → 快速过一眼 JSON
08:16  复制完整 JSON
08:17  打开 GitHub 仓库 → src/data/data.json → 编辑 → 全选粘贴 → Commit
08:19  Cloudflare Pages 自动重新部署
08:20  网页已更新，团队 9:00 上班看到的是当日新数据
```

### 手动刷新兜底

如果某天定时任务没跑，点击网页右上角「🔄 手动刷新数据」→ 复制 Prompt → 到任意 Claude 对话粘贴运行。

---

## 📱 9 个平台 Prompt 设计原则（已更新）

每个选题卡片下方有 9 个平台按钮，点击复制该平台的专属 Prompt。**X 不在这 9 个按钮里**，是CIO办公室日报的专属输出通道，内容基本来自CIO日报本身，是否发布由Doris独立判断，不走选题雷达的系统化流程。

### 两类内容目标（新增，决定选题往哪个方向做）

对外内容有两个目标，选题时先判断这条题服务哪一个，再决定往哪几个平台配：

| 目标 | 内容形态 | 主要承接平台 | 判断标准 |
|---|---|---|---|
| **认知 / AI可见性** | 深度专业、有数据锚点、讲逻辑 | 官网、LinkedIn、YouTube、Medium | 是否命中诺亚产品线/CIO观点，专业度要求高 |
| **客户转化** | 轻松、热点、生活化，借大众话题包一层财富启示 | Facebook（软性入口）→ 官网链接/落地页 | 是否有天然的CTA路径，不强求专业深度 |

转化类选题的典型形态：借一个大众正在讨论的话题（热播剧、社会新闻、生活方式热点）给出几条财富管理角度的启示，同行私行也有过用热播剧讲财富传承的软性内容案例，这类题目此前容易因"专业度不够"被过滤掉，需要单独放行。

转化链路：**海外社媒（Facebook等软性内容）→ 官网链接/落地页（放置WhatsApp咨询入口）→ 客户主动咨询**。官网内容体系需要同时承接认知类深度长文和转化类落地页两种出口。

### 9 个平台清单（已更新，替换旧的微信公众号/知乎/X）

> 微信讲逻辑 → **改为官网讲逻辑** · 小红书讲情绪 · Facebook讲软性转化 · LinkedIn讲权威 · 知乎讲深度 → **改为Medium讲深度**
> Reddit / Quora 讲客观专业 · YouTube / 视频号讲叙事

每个 Prompt 包含三层结构：
- **背景注入**（诺亚品牌信息，共用以约 200 字）
- **话题注入**（动态填充选题数据）
- **平台定制指令**（每个平台独立，是差异化关键）

平台按钮顺序（按编号固定，不要调整）：

1. **官网**（原微信公众号位，无公众号故替换）— 深度长文，讲逻辑、有数据，服务认知目标
2. **小红书** — 3 版图文笔记，情绪向、生活化
3. **视频号** — 60-90 秒口播脚本，开门见山
4. **Facebook**（原X位，定位已修正）— **软性、生活化、偏营销转化导向**，末尾自然带官网链接或落地页，服务转化目标
5. **LinkedIn** — 高管署名长文，中英双语，服务认知目标
6. **Medium**（原知乎位，无知乎故替换）— 2000-3000 字专业长文，服务认知目标
7. **YouTube** — 8-12 分钟脚本，含章节时间戳
8. **Reddit** — 英文帖子草稿，客观专业
9. **Quora** — 3 个英文问答，专家口吻

**X（独立于9平台之外）**：CIO日报专属发布口，内容基本来自CIO办公室日报，不走选题雷达的选题→生成流程，由Doris自行判断是否发布及发布内容。

---

## 🎬 内容创作工作流

策划小 A 的一个典型早晨：

```
09:00  打开 https://noah-radar.pages.dev
09:02  在"选题生成器"模块看到今日选题池，挑中一个
09:03  先判断这条题服务认知还是转化，再点对应平台按钮
09:04  切换到 Claude 对话，粘贴、回车
09:05  Claude 输出对应平台的初稿
09:15  再点同一选题的另一个平台按钮 → 复制 → 粘贴 → 输出对应内容
09:20  多个平台的内容已经全有了
```

整个"选题 → 多平台初稿"只用了 20 分钟。

---

## 🛠️ 常见维护操作

### 调整平台按钮生成的 Prompt

改 `src/prompts.js`。所有 Prompt 都在这里统一管理。**以下函数名需要根据新平台清单重命名并更新内容**：

| 函数名（需更新） | 对应按钮 | 变更说明 |
|---|---|---|
| ~~`buildPromptWeChat`~~ → `buildPromptWebsite` | 选题卡 → 官网 | 原微信公众号位替换为官网 |
| `buildPromptXiaohongshu` | 选题卡 → 小红书 | 不变 |
| `buildPromptVideoChannel` | 选题卡 → 视频号 | 不变 |
| ~~`buildPromptX`~~ → `buildPromptFacebook` | 选题卡 → Facebook | 原X位替换为Facebook，且Prompt指令需改为软性/转化导向，不再是"犀利可引用" |
| `buildPromptLinkedIn` | 选题卡 → LinkedIn | 不变 |
| ~~`buildPromptZhihu`~~ → `buildPromptMedium` | 选题卡 → Medium | 原知乎位替换为Medium |
| `buildPromptYouTube` | 选题卡 → YouTube | 不变 |
| `buildPromptReddit` | 选题卡 → Reddit | 不变 |
| `buildPromptQuora` | 选题卡 → Quora | 不变 |
| `buildBriefingPackagePrompt` | 今日早报 → 生成内容物料包 | 不变 |
| `buildNewsToTopicPrompt` | 新闻 → 策划成选题 | 不变 |
| `buildCalendarAnglesPrompt` | 日历节点 → 展开切入角度 | 不变 |
| `DAILY_CRAWL_PROMPT` | 顶部"手动刷新"弹窗 | 不变 |
| （新增）`buildPromptXCIO` | 独立CIO日报通道，不挂在选题卡下 | 新增：X的Prompt改为独立入口，输入来源是CIO日报而非选题池 |

改完 push 到 GitHub，Cloudflare Pages 自动重新部署。

### 修改颜色主题

所有主色用了设计 token：
- `#8B6F2A` = 主金色
- `#7A5D20` = 辅助金色
- `#1A1F2E` = 主文字墨色
- `#4A5268` = 次文字
- `#8A8578` = 辅助文字
- `#F7F3EA` / `#FDFBF5` = 两档象牙白背景

### 修改诺亚 Logo

`src/App.jsx` 顶部的 `NOAH_LOGO` 常量是 base64 编码图片。替换 base64 字符串即可。

---

## ❓ 常见问题

**Q: 复制按钮不工作？**
A: 检查浏览器是否授予了剪贴板权限。Cloudflare Pages 的 https 域名下默认允许。

**Q: 网页打不开 / 空白？**
A: 打开浏览器 F12 控制台看报错。常见原因：`data.json` 格式错误。用 [jsonlint.com](https://jsonlint.com) 校验一下。

**Q: 可以让非诺亚员工看到吗？**
A: 可以，把 Cloudflare Access 里添加外部邮箱即可。但建议只给内部团队。

**Q: 数据每天手动更新很麻烦，能不能全自动？**
A: 当前用 Claude 定时任务方案需手动复制粘贴到 GitHub，每天约 2 分钟。如需全自动，可在 Cloudflare 加 Worker 每日 8:00 调 Claude API 生成 + GitHub API 提交，月成本约 ¥300-500。**本次 v2.1 保留扩展性**，所有 Prompt 在 `prompts.js` 中统一管理，未来 Worker 可直接 import 复用。

**Q: 9 个平台的 Prompt 能调整吗？**
A: 当然。`src/prompts.js` 里每个 `buildPrompt[平台名]` 函数对应一个平台的 Prompt，改里面的平台指令即可。

**Q: X为什么不在9平台按钮里？**
A: X是CIO办公室日报的专属发布口，内容基本来自日报本身，不适合走"选题池→9平台一键生成"的通用流程，由Doris独立判断发布内容和时机。

---

## 📞 迭代与支持

遇到问题、想加新功能、调整 Prompt 风格，直接在 Claude 对话里告诉我。这个项目的设计理念就是"让 AI 持续陪伴迭代"。

**版本**：v2.1 · 全平台选题中枢（平台清单修正 + 内容目标分类）
**最后更新**：2026-09-15
