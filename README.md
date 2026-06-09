# 🔥 Obsidian Nirvana Handbook (完整包 / Complete Package)

[English](Obsidian_Nirvana_Handbook.md) | [中文](Obsidian%20%E6%B6%85%E6%A7%83%E6%89%8B%E5%86%8C.md)

> **One-sentence summary**: Turn dead text in Obsidian into living presentations, then infuse them with the soul of LiveView — let your notes live forever in the browser.

---

## 📦 What's Inside

This package contains the complete **Obsidian → Marp → Phoenix LiveView** migration toolchain, including:

- **Chinese Handbook** (`Obsidian 涅槃手册.md`) — Full guide with WSL setup, interactive backgrounds, and adaptation checklists
- **English Handbook** (`Obsidian_Nirvana_Handbook.md`) — Complete translation
- **Skill Library** (`skills/`) — Reusable skill files for every stage of the pipeline

---

## 🗂️ Directory Structure

```
Obsidian涅槃手册_完整包/
├── README.md                                    # This file
├── Obsidian 涅槃手册.md                          # Chinese handbook
├── Obsidian_Nirvana_Handbook.md                 # English handbook
└── skills/                                      # Reusable skill library
    ├── Marp_to_LiveView_SKILL.md               # Marp HTML → LiveView migration
    ├── Elixir_Phoenix_LiveView_Ecosystem_SKILL.md  # OTP / LiveView / HEEx / ETS reference
    ├── Phoenix_LiveView_3D_Block_Wave_SKILL.md # 3D background + page transitions
    ├── Interactive_Background_Alchemist_SKILL.md   # Canvas effect → Skill → LiveView integration
    └── Canvas_Effects_Catalog_SKILL.md         # Verified background effects catalog
```

---

## 🚀 Quick Start

### For Chinese Readers

直接阅读 [`Obsidian 涅槃手册.md`](Obsidian%20%E6%B6%85%E6%A7%83%E6%89%8B%E5%86%8C.md)，按五阶段流程操作：

1. **第一阶段** — Obsidian 原料预处理
2. **第二阶段** — Agent Marp 适配
3. **第三阶段** — Marp CLI 导出 HTML
4. **第四阶段** — WSL 环境下 Elixir + Phoenix + LiveView 化
5. **第五阶段** — 交互化背景注入

### For English Readers

Read [`Obsidian_Nirvana_Handbook.md`](Obsidian_Nirvana_Handbook.md) and follow the five-stage pipeline:

1. **Stage 1** — Obsidian Raw Material Preprocessing
2. **Stage 2** — Agent Marp Adaptation
3. **Stage 3** — Marp CLI HTML Export
4. **Stage 4** — Elixir + Phoenix + LiveView in WSL
5. **Stage 5** — Interactive Background Injection

---

## 🛠️ Skill Library Usage

All skill files in `skills/` are **reference-only** — they contain architecture guidance, checklists, and code templates. Do not copy entire code blocks blindly; instead, open the relevant skill and follow its chapter-by-chapter instructions.

| Skill | Purpose | When to Use |
|-------|---------|-------------|
| `Marp_to_LiveView_SKILL.md` | Full migration path from static HTML to dynamic LiveView | After exporting Marp HTML |
| `Elixir_Phoenix_LiveView_Ecosystem_SKILL.md` | Full-stack Elixir ecosystem reference | When writing business logic, UI, or database code |
| `Phoenix_LiveView_3D_Block_Wave_SKILL.md` | Three.js instanced rendering + custom shaders | When you need a high-impact 3D background |
| `Interactive_Background_Alchemist_SKILL.md` | Canvas effect → reusable Skill → LiveView Hook | When you want to create or install a new background effect |
| `Canvas_Effects_Catalog_SKILL.md` | Quick lookup of all verified background effects | When selecting a background for your slides |

---

## 📋 Prerequisites

- **Obsidian** with community plugins (Templater, QuickAdd, Linter recommended)
- **Node.js** 18+ and `@marp-team/marp-cli`
- **WSL2** with Ubuntu (or native Linux/macOS)
- **Elixir** 1.15+ and **Erlang** 26+ (via `asdf` recommended)
- **Phoenix** 1.7+ (`mix archive.install hex phx_new`)

---

## 🤝 Contributing

This is a personal knowledge-management toolchain. Feel free to fork and adapt to your own vault structure. If you smelt new Canvas effects into Skills, add them to `skills/` and update `Canvas_Effects_Catalog_SKILL.md`.

---

## 📜 License

MIT — Use freely, modify boldly, share generously.

---

> **"让每一篇笔记，都有机会站上舞台。"** 🎭✨
>
> **"Let every note have a chance to stand on stage."**
