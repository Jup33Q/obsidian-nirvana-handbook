# 🔥 Obsidian 涅槃手册：从笔记到 LiveView 的三阶进化

> **一句话概括**：把 Obsidian 里的「死文字」炼成「活演示」，再注入 LiveView 的灵魂，让笔记在浏览器里永生。

---

## 📋 流程总览

```
┌─────────────┐    ┌─────────────┐    ┌─────────────┐    ┌─────────────────────┐
│  Obsidian   │ ──▶│ Marp 适配   │ ──▶│  HTML 导出  │ ──▶│ Phoenix + LiveView  │
│  原始笔记   │    │ Agent 处理  │    │ Marp CLI    │    │ WSL 环境下运行      │
└─────────────┘    └─────────────┘    └─────────────┘    └─────────────────────┘
      │                   │                  │                     │
      ▼                   ▼                  ▼                     ▼
  Markdown 原文      幻灯片 Markdown    静态 HTML 文件        实时交互 Web 应用
  + YAML Frontmatter + 主题 + 布局      + CSS + 图片           + WebSocket 推送
                                                            + 交互化背景注入
```

---

## 🧱 第一阶段：Obsidian 原料预处理

### 1.1 素材要求

你的 Obsidian 笔记需要满足以下条件，才能进入涅槃流程：

| 检查项 | 要求 | 目的 |
|--------|------|------|
| YAML Frontmatter | 必须有 `title`、`tags`、`date` | 供后续 Agent 识别主题与元信息 |
| 图片引用 | 使用 WikiLink `![[image.png]]` 或标准 Markdown `![](path)` | Marp 能正确解析并内嵌 |
| 代码块 | 使用 ``` 包裹并标注语言 | 导出后高亮样式完整保留 |
| 层级结构 | H1-H3 清晰分节 | 自动适配幻灯片分页逻辑 |

### 1.2 文件夹结构建议

```
Obsidian Vault/
├── 01-笔记原料/           # 原始笔记，随意写
│   ├── 项目复盘-2024Q4.md
│   └── 技术分享-Kafka.md
├── 02-涅槃中间件/         # Agent 处理后的 Marp 适配稿
│   └── (由 Agent 自动生成)
├── 03-幻灯片成品/         # Marp CLI 导出的 HTML/PDF
│   └── (由 Marp CLI 自动生成)
└── 04-web-live/           # Phoenix LiveView 项目
    └── (由 Elixir 代码接管)
```

### 1.3 相关 Skill 引用

Obsidian 侧的工具链可直接调用以下已安装 Skill：

| 能力 | 推荐 Skill | 用途 |
|------|-----------|------|
| 模板快速导出 | `obsidian-templater` | 一键将笔记注入中间件目录 |
| 批量创建笔记 | `obsidian-quickadd` | 标准化新建幻灯片源文件 |
| 元数据规范化 | `obsidian-frontmatter-standard` | 统一 YAML 字段格式 |
| 图片本地归档 | `obsidian-local-images-plus` | 外链图片转本地，避免 Marp 导出断链 |
| 格式一键修复 | `obsidian-linter` | 批量标准化 Markdown 格式 |

> 💡 **一句话**：先在 Obsidian 里把笔记「养肥」，再送进涅槃炉。

---

## 🤖 第二阶段：Agent Marp 适配（炼金术核心）

### 2.1 交给 Agent 的 Prompt 模板

```text
你是一位 Marp 幻灯片架构师。请将以下 Obsidian 笔记转换为符合 Marp 语法的 Markdown。

## 输入
- 原始 Obsidian Markdown（见下方）
- 主题偏好：{{theme}}（如 default、gaia、uncover）
- 分页策略：{{strategy}}（如 per-H2、auto、manual）

## 输出要求
1. 在 Frontmatter 中插入 Marp 配置：
   ---
   marp: true
   theme: {{theme}}
   paginate: true
   ---

2. 自动在 H2 前插入分页符 `---`（按策略）
3. 将 WikiLink 图片转换为相对路径 `![](../assets/xxx.png)`
4. 代码块超过 20 行时自动折叠或分片显示
5. 生成演讲者备注 `<!-- 备注内容 -->` 在每页底部

## 原始内容
{{obsidian_raw_content}}
```

### 2.2 批量处理思路

将 `01-笔记原料/` 批量喂给 Agent，输出到 `02-涅槃中间件/marp-ready/`。可用任何自动化方式（GitHub Actions、本地脚本、或直接在对话中逐篇处理）。

核心原则：Agent 只负责**语法转换**（Obsidian Markdown → Marp Markdown），不涉及样式设计。

---

## 🎨 第三阶段：Marp CLI 导出 HTML

### 3.1 安装与导出

```bash
# 安装
npm install -g @marp-team/marp-cli

# 单文件导出
marp input.md --html --allow-local-files --bundle -o output.html

# 批量导出
marp --config-file marp.config.js *.md
```

关键参数：
- `--html`：允许 HTML 标签（后续 LiveView 嵌入必需）
- `--allow-local-files`：解析本地图片
- `--bundle`：内嵌所有资源为单文件 HTML，方便传输

### 3.2 导出产物

```
03-幻灯片成品/
├── 项目复盘-2024Q4.html      # 单文件（含内嵌 CSS/图片）
├── 技术分享-Kafka.html
└── assets/                    # 若未 bundle，资源外置
```

---

## 🔥 第四阶段：WSL 环境下 Elixir + Phoenix + LiveView 化

> 这是涅槃的最后一步：把静态 HTML 幻灯片变成**可交互、可控制、可多人协作**的实时 Web 应用。

### 4.1 环境准备（WSL2 Ubuntu）

```bash
# 进入 WSL
wsl ~

# Elixir / Erlang（推荐 asdf 版本管理）
asdf install erlang latest
asdf install elixir latest

# Phoenix
mix local.hex
mix archive.install hex phx_new

# 新建项目（如需数据库）
mix phx.new SlideLive
# 或不需要数据库
mix phx.new SlideLive --no-ecto --no-mailer
```

### 4.2 项目导入 WSL 后的初始化

当你把现有 Phoenix 项目（如从 Windows 侧复制或 git clone 到 WSL）后，必须执行以下初始化步骤：

```bash
# 1. 进入项目目录
cd /path/to/slider_live_demo

# 2. 安装 Elixir 依赖
mix deps.get

# 3. 创建并迁移数据库（若项目使用 Ecto）
mix ecto.setup
# 或分开执行：
# mix ecto.create && mix ecto.migrate && mix run priv/repo/seeds.exs

# 4. 安装 Node 依赖（assets 目录）
cd assets && npm install && cd ..

# 5. 构建前端资源（开发模式，esbuild + tailwind 会自动监听）
# 首次启动前建议手动编译一次
mix assets.build

# 6. 启动 Phoenix 服务器
mix phx.server
# 或带交互式 shell
iex -S mix phx.server
```

#### WSL 网络访问注意

| 访问方式 | 地址 | 说明 |
|---------|------|------|
| WSL 内部浏览器 | `http://localhost:4000` | 直接在 WSL 内安装 lynx/chromium 时可用 |
| Windows 浏览器 | `http://localhost:4000` | WSL2 自动转发端口，最常用 |
| 局域网其他设备 | `http://<WSL_IP>:4000` | `wsl hostname -I` 获取 IP |

> ⚠️ 若 Windows 侧访问不了，检查 Windows Defender 防火墙或 WSL 的 `autoProxy` 设置。

#### 开发常用命令速查

```bash
# 重置数据库（开发时清空重来）
mix ecto.reset

# 格式化代码（提交前必做）
mix format

# 运行测试
mix test

# 生产构建（部署前）
mix assets.deploy

# 生成新 LiveView
mix phx.gen.live Slides Slide slides number:integer title:string
```

### 4.3 核心：引用 Marp → LiveView Skill

**不要从零写代码。** 直接调用已安装的 Skill：

📄 **Skill 位置**：`skills/Marp_to_LiveView_SKILL.md`

该 Skill 覆盖了从 Marp HTML 到 LiveView 的完整迁移路径，包含：

| 模块 | Skill 内对应章节 | 说明 |
|------|----------------|------|
| HTML 问题诊断 | `问题诊断清单 P1-P7` | 检查 CSS 内联、SVG 包装、CDN 依赖等 |
| 解析层 | `迁移架构 → Parser Layer` | Marpit API / Earmark 提取 {html, css, comments} |
| 状态层 | `slide_deck_live.ex 模板` | LiveView 核心：slide 索引、主题、转场状态 |
| 交互层 | `SlideTouch Hook` | 键盘/触摸/滚轮导航 |
| 可视化层 | `ECharts Hook` | 表格数据自动转图表（可选） |
| 协作层 | `协作扩展` | PubSub + Presence 多人同步 |
| 性能清单 | `性能检查清单` | bare 模板、零内联 style、零外部 CDN 等 |

**使用方式**：打开该 Skill 文件，按章节复制对应模板到你的 Phoenix 项目即可。

### 4.4 Elixir / Phoenix 生态速查

**Skill 位置**：`skills/Elixir_Phoenix_LiveView_Ecosystem_SKILL.md`

该 Skill 提供全栈参考：

| 需求 | 查看章节 |
|------|---------|
| GenServer / Supervisor 进程树 | `references/otp.md` |
| LiveView 状态管理（mount, handle_event, assigns） | `references/liveview-core.md` |
| HEEx + Tailwind + DaisyUI 组件样式 | `references/liveview-ui-heex.md` |
| ETS 内存缓存（幻灯片数据暂存） | `references/ets.md` |
| Ecto + SQLite3（如需持久化投票/问卷数据） | `references/ecto-sqlite.md` |
| 宏与 DSL 扩展 | `references/metaprogramming.md` |
| 音视频流（如需嵌入直播/录屏） | `references/membrane.md` |

### 4.5 适配粗略步骤（Checklist）

将 Marp HTML 迁移到 Phoenix LiveView 时的最小必要步骤：

```
□ 1. 在 WSL 中初始化项目（见 4.2）
□ 2. 运行 mix test，确保基线通过
□ 3. 创建 Slide Ecto Schema + Migration + Context
□ 4. 编写 priv/repo/seeds.exs，将 Marp 幻灯片数据灌入 DB
□ 5. 创建 SlideDeckLive（单页模式，push_navigate 翻页）
□ 6. 将 Marp HTML 的 CSS 去内联化，迁入 assets/css/app.css
□ 7. 替换 SVG foreignObject 包装为纯 HTML <div class="slide">
□ 8. 创建 BubbleQuilt / TransitionFlash JS Hook（或引用已有 Skill）
□ 9. 添加键盘事件监听（← → 翻页，ESC 回首页）
□ 10. 配置 Cookie 记住 last_slide，实现刷新不丢位置
□ 11. 检查并移除所有外部 CDN 依赖（twemoji 等）
□ 12. 添加 prefers-reduced-motion 媒体查询
□ 13. 再次运行 mix test，确保全绿
□ 14. Windows 浏览器访问 localhost:4000 验证效果
```

> 📌 每个步骤的详细代码模板均可在 `skills/Marp_to_LiveView_SKILL.md` 中找到对应章节。

---

## 🌌 第五阶段：交互化背景注入（灵魂觉醒）

> 静态幻灯片只是躯壳，交互化背景才是灵魂。本阶段教你如何把「随手生成的 Canvas 特效」炼成可复用的 Skill，并注入 LiveView。

### 5.1 整体流程

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│  向 Agent 描述  │ ──▶│  Agent 生成代码  │ ──▶│  熔炼成 Skill   │ ──▶│  LiveView 集成  │
│  想要的背景效果  │    │  (HTML/JS/TS)   │    │  (SKILL.md+assets)│   │  (Hook + HEEx)  │
└─────────────────┘    └─────────────────┘    └─────────────────┘    └─────────────────┘
        召唤                  生成                 封装                 附魔
```

### 5.2 召唤：向 Kimi Agent 生成效果

使用 Prompt 模板向任意 Kimi Agent 发起请求，描述你想要的背景：

```text
请为我生成一个 Canvas 2D 交互背景特效，用于 Phoenix LiveView 幻灯片页面。

## 需求描述
- 主题：{{主题}}
- 交互：{{交互方式}}
- 色调：{{主色调 + 背景色}}
- 性能要求：60fps @ 1080p
- 输出形式：单文件 vanilla JS，无框架依赖

## 输出要求
1. 可直接运行的 .html 演示文件
2. 剥离出的纯 .js 文件
3. 100 字以内的效果描述
4. 配置参数表（颜色、粒子数、速度等）
```

### 5.3 生成 → Skill → 安装

**不要手动复制粘贴整段代码。** 完整流程封装在以下 Skill 中：

📄 **Skill 位置**：`skills/Interactive_Background_Alchemist_SKILL.md`

该 Skill 包含：

| 阶段 | 对应章节 | 说明 |
|------|---------|------|
| 召唤 | `第一阶段：向 Agent 生成效果` | Prompt 模板 + 推荐描述关键词 |
| 生成 | `第二阶段：熔炼成 Skill` | 目录结构、SKILL.md 头信息、JS 接口规范、熔炼检查清单 |
| 封装 | `第三阶段：安装 Skill` | 用户级 / 项目级 / 手册包 三种安装方式 |
| 附魔 | `第四阶段：LiveView 集成` | JS Hook 模板、Hook 注册、HEEx 引用、多背景切换策略 |

### 5.4 已有效果速查与选型

所有已验证可用的交互背景 Skill 汇总在：

📄 **Skill 位置**：`skills/Canvas_Effects_Catalog_SKILL.md`

| Skill 名称 | 效果类型 | 技术栈 | 性能 | 适用场景 |
|-----------|---------|--------|------|---------|
| `canvas-effect-aurora` | 极光呼吸场 | Canvas 2D, 36 光带 | 极低 | 科技风、深色主题 |
| `canvas-effect-nebula` | 星云粒子 | Canvas 2D, 物理引擎 | 中等 | 数据大屏、星空主题 |
| `canvas-effect-firefly` | 萤火虫星座 | Canvas 2D, 连线算法 | 中等 | 自然主题、夜间氛围 |
| `canvas-effect-liquid` | 液态光绘 | Canvas 2D, Bezier | 中高 | 创意工具、艺术主题 |
| `canvas-effect-geometric` | 几何矩阵 | Canvas 2D, 网格变换 | 高 | 极简主义、结构感 |
| `ide-code-wave-bg` | 代码字符波 | Canvas 2D, 语法高亮 | 中等 | 开发者作品集 |
| `phoenix_liveview_3d_block_wave` | 3D 方块浪 | Three.js, 自定义着色器 | 高 | 品牌展示、发布会 |
| `make-marp-html-bubble` | 气泡拼布 | Canvas 2D, 物理碰撞 | 中等 | Marp HTML 专用 |

> 💡 **选型技巧**：打开 `Canvas_Effects_Catalog_SKILL.md`，按「选型决策树」快速定位最适合当前幻灯片主题的背景效果。

### 5.5 在 LiveView 中挂载背景（一句话）

```heex
<canvas phx-hook="Aurora" class="fixed inset-0 -z-10 pointer-events-none"></canvas>
```

具体 Hook 编写和注册方式，参见 `Interactive_Background_Alchemist_SKILL.md` 的「第四阶段」。

---

## 🚀 进阶玩法（均可在对应 Skill 中找到实现模板）

| 玩法 | 依赖 Skill | 核心思路 |
|------|-----------|---------|
| **多人协作演讲** | `Marp_to_LiveView_SKILL.md` → 协作扩展章节 | Presence 追踪观众，PubSub 广播翻页 |
| **实时投票 / 问答** | `Elixir_Phoenix_LiveView_Ecosystem_SKILL.md` → liveview-core.md | LiveComponent + ETS 计数 |
| **URL 同步当前页** | `Marp_to_LiveView_SKILL.md` → `handle_params/3 + push_patch/2` | 刷新不丢位置，可分享直达某页 |
| **语音控制翻页** | 自行集成 Whisper API | `handle_event("voice_command", ...)` |
| **3D 波浪背景** | `Phoenix_LiveView_3D_Block_Wave_SKILL.md` | Three.js Hook + 自定义着色器 |
| **多背景自动切换** | `Interactive_Background_Alchemist_SKILL.md` → 多背景切换策略 | 按幻灯片页号或主题动态换背景 |

---

## 🛠️ 故障排查速查表

| 现象 | 可能原因 | 解决 |
|------|----------|------|
| Marp 图片不显示 | 路径错误或缺 `--allow-local-files` | 检查相对路径，加 `--allow-local-files` |
| LiveView 不更新 | WebSocket 被代理阻断 | 检查 Nginx/防火墙 ws:// 转发配置 |
| WSL 端口无法访问 | WSL 网络隔离 | `host.docker.internal` 或 WSL 内访问 localhost |
| 幻灯片样式丢失 | Marp 主题未正确导出 | `--bundle` 内嵌 CSS，或确保主题文件存在 |
| Agent 分页混乱 | H2 层级不统一 | Obsidian 中规范标题层级，或改用 manual 分页 |
| HTML 文件体积过大 | CSS 重复内联 | 参见 `Marp_to_LiveView_SKILL.md` P1 修复方案 |
| Emoji 离线不显示 | 外部 CDN 依赖 | 参见 `Marp_to_LiveView_SKILL.md` Emoji 组件章节 |
| mix deps.get 失败 | Hex 镜像或网络问题 | `mix hex.config mirror_url https://...` |
| ecto.setup 报错 | SQLite3 未安装 | `sudo apt install sqlite3 libsqlite3-dev` |
| assets 编译失败 | Node 版本过低 | 升级 Node 到 18+，`nvm install 20` |
| Canvas 背景不显示 | Hook 未注册或 canvas 尺寸为 0 | 检查 `app.js` hooks 对象 + CSS `w-full h-full` |

---

## 📚 工具链与 Skill 映射总表

| 层级 | 工具 / Skill | 用途 |
|------|-------------|------|
| 笔记层 | Obsidian + Templater/QuickAdd | 原始内容生产 |
| | `obsidian-frontmatter-standard` | YAML 字段规范 |
| | `obsidian-local-images-plus` | 图片本地化 |
| | `obsidian-linter` | 格式标准化 |
| 适配层 | AI Agent | Markdown → Marp 语法转换 |
| 导出层 | Marp CLI | Marp → HTML/PDF |
| 运行时 | WSL2 + Ubuntu | Linux 子系统环境 |
| Web 层 | `skills/Marp_to_LiveView_SKILL.md` | Marp HTML → LiveView 迁移 |
| | `skills/Elixir_Phoenix_LiveView_Ecosystem_SKILL.md` | OTP / LiveView / HEEx / ETS 全栈参考 |
| 背景层 | `skills/Interactive_Background_Alchemist_SKILL.md` | 交互背景生成 → Skill → 集成全流程 |
| | `skills/Canvas_Effects_Catalog_SKILL.md` | 已可用背景效果速查与选型 |
| | `skills/Phoenix_LiveView_3D_Block_Wave_SKILL.md` | 3D 背景 + 页面过渡 |
| 部署层 | systemd / Docker / fly.io | 长期运行托管 |

---

## 💡 命名哲学

> **为什么叫「Obsidian 涅槃手册」？**
>
> - **Obsidian**：你的知识原料，黑曜石般的原始矿石。
> - **涅槃**：佛教中的「圆寂重生」——笔记经历三阶烈焰焚烧，从静态 Markdown 死物，化为 LiveView 中呼吸、互动、与人共生的生命体。
> - **手册**：不是一次性脚本，而是一本可随时翻阅、按需调用、持续迭代的操作指南。
>
> 三阶烈焰：
> 1. **Obsidian → Marp**：从「写给自己看」到「讲给别人听」
> 2. **Marp → HTML**：从「本地文件」到「可分发格式」
> 3. **HTML → LiveView**：从「单向广播」到「实时互动」
> 4. **LiveView → 交互背景**：从「黑白默片」到「声色俱全」

---

## 🔮 未来扩展（Roadmap）

- [ ] **语音控制**：集成 Whisper，喊「下一页」自动翻片
- [ ] **AI 实时解说**：根据当前幻灯片内容，由 LLM 生成口述稿
- [ ] **情绪分析**：通过摄像头捕捉观众表情，实时调整演讲节奏
- [ ] **区块链存证**：每次演讲上链，生成不可篡改的分享记录（开玩笑的 😄）

---

> **"让每一篇笔记，都有机会站上舞台。"** 🎭✨
