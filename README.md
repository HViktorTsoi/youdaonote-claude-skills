# YoudaoNote Claude Code Skills

**有道云笔记 × Claude Code —— 用自然语言操作你的笔记**

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Claude Code](https://img.shields.io/badge/Claude%20Code-Skill-blue)](https://claude.ai/code)
[![Platform](https://img.shields.io/badge/Platform-macOS%20%7C%20Linux-lightgrey)](https://note.youdao.com)

---

> **给人类用户的提示：** 把这个页面的链接或内容直接发给你的 Claude Code，说「帮我安装这个 skill」，它会自动完成所有配置步骤。
>
> **For human users:** Share this page's URL or content with your Claude Code and say "help me install this skill" — it will handle the entire setup automatically.

---

[English](#english) | [中文](#中文)

---

## 中文

### 简介

本项目为 [Claude Code](https://claude.ai/code) 提供两个有道云笔记 Skill，让你直接在 Claude Code 对话中用自然语言管理笔记、剪藏网页，无需切换 App。

| Skill | 功能 |
|-------|------|
| `youdaonote-notes` | 列出 / 读取 / 创建 / 更新 / 删除 / 搜索笔记，管理待办事项 |
| `youdaonote-clip` | 网页剪藏、查看最近收藏 |

### 效果演示

```
你：帮我列出"工作"目录里的笔记
Claude：📁 项目A  📄 想法.note  📄 会议摘要.note ...

你：把这篇文章保存到有道云笔记 https://arxiv.org/abs/2404.xxxxx
Claude：✅ 已剪藏「XXX: A Survey」到有道云笔记

你：在"归档"目录下创建一篇今日会议记录
Claude：✅ 已创建笔记「2026-04-01 会议记录」
```

---

### AI Agent 自动配置指南

> 本节专为 AI agent 设计，按顺序执行以下步骤即可完成全部配置。

#### 步骤 1：检查并安装 youdaonote CLI

```bash
# 检查是否已安装
which youdaonote || ~/.local/bin/youdaonote --version
```

- **若命令存在** → 跳至步骤 2
- **若命令不存在** → 执行安装：

```bash
curl -fsSL https://artifact.lx.netease.com/download/youdaonote-cli/install.sh | bash
```

安装后使用完整路径调用（当前终端 PATH 尚未刷新）：

```bash
~/.local/bin/youdaonote --version
```

**验证成功标志：** 输出版本号，例如 `1.2.0`

---

#### 步骤 2：配置 API Key

**在执行此步骤前，必须先询问用户：**

> 请前往 https://mopen.163.com/#/dashboard 获取有道云笔记 API Key（需登录并绑定手机号），获取后告诉我。

用户提供 Key 后执行：

```bash
~/.local/bin/youdaonote config set apiKey <用户提供的KEY>
```

**验证成功标志：** 输出 `已写入 mcp.apiKey`

---

#### 步骤 3：验证连接

```bash
~/.local/bin/youdaonote check
```

**验证成功标志：** 所有项目显示 `✅`，最后一行为 `✅ 所有检查通过`

- **若出现 `❌ api-key`** → API Key 有误，返回步骤 2 重新获取
- **若出现 `❌ mcp-connection`** → 网络问题，提示用户检查网络后重试

---

#### 步骤 4：安装 Skills 文件

```bash
# 创建目标目录（如不存在）
mkdir -p ~/.claude/skills/youdaonote-notes ~/.claude/skills/youdaonote-clip

# 下载 skill 文件
curl -fsSL https://raw.githubusercontent.com/HViktorTsoi/youdaonote-claude-skills/main/skills/youdaonote-notes/SKILL.md \
  -o ~/.claude/skills/youdaonote-notes/SKILL.md

curl -fsSL https://raw.githubusercontent.com/HViktorTsoi/youdaonote-claude-skills/main/skills/youdaonote-clip/SKILL.md \
  -o ~/.claude/skills/youdaonote-clip/SKILL.md
```

**验证成功标志：**

```bash
ls ~/.claude/skills/youdaonote-notes/SKILL.md ~/.claude/skills/youdaonote-clip/SKILL.md
```

两个文件均存在则安装完成。

---

#### 步骤 5：收尾确认

告知用户：

> Skills 已安装完成！请**重启 Claude Code**，之后即可直接对话操作有道云笔记，例如：
> - 「列出我的有道云笔记」
> - 「帮我剪藏这个链接 https://...」

---

### 手动安装

如需手动安装（不依赖 AI agent），完整步骤如下：

#### 第一步：安装有道云笔记 CLI

```bash
curl -fsSL https://artifact.lx.netease.com/download/youdaonote-cli/install.sh | bash
```

安装路径：`~/.local/bin/youdaonote`，配置文件：`~/.youdaonote.json`

#### 第二步：配置 API Key

前往 [网易智能开发者平台](https://mopen.163.com/#/dashboard) 获取 API Key（需绑定手机号的有道云笔记账号）：

```bash
youdaonote config set apiKey YOUR_API_KEY

# 验证连接
youdaonote check
```

#### 第三步：安装 Skills

```bash
mkdir -p ~/.claude/skills/youdaonote-notes ~/.claude/skills/youdaonote-clip

curl -fsSL https://raw.githubusercontent.com/HViktorTsoi/youdaonote-claude-skills/main/skills/youdaonote-notes/SKILL.md \
  -o ~/.claude/skills/youdaonote-notes/SKILL.md

curl -fsSL https://raw.githubusercontent.com/HViktorTsoi/youdaonote-claude-skills/main/skills/youdaonote-clip/SKILL.md \
  -o ~/.claude/skills/youdaonote-clip/SKILL.md
```

安装完成后，重启 Claude Code，Skills 即自动生效。

---

### 使用示例

#### 笔记管理

```
列出我的有道云笔记
帮我搜索笔记里关于"深度学习"的内容
在"工作"目录下新建一篇笔记，标题是"会议记录"
读取笔记 <fileId> 的内容
把笔记重命名为"新标题"
```

#### 网页剪藏

```
帮我剪藏这篇文章 https://example.com/article
查看我最近收藏的 20 篇文章
把这个链接保存到有道云笔记的"学习"目录
```

#### 待办事项

```
列出我的所有待办事项
新建一个待办：明天完成季度报告，截止日期 2026-04-10
把待办事项 <todoId> 标记为完成
```

### 目录结构

```
youdaonote-claude-skills/
├── README.md
├── LICENSE
├── skills/
│   ├── youdaonote-notes/
│   │   └── SKILL.md          # 笔记管理 + Todo
│   └── youdaonote-clip/
│       └── SKILL.md          # 网页剪藏
└── examples/
    ├── note-management.md    # 笔记管理示例
    └── web-clipping.md       # 网页剪藏示例
```

### 常见问题

**Q: 提示"命令未找到"**
先尝试完整路径：`~/.local/bin/youdaonote`。若仍失败，重新执行安装脚本。

**Q: 创建的笔记无法更新**
`update` 命令仅支持 `.md` 格式笔记。富文本 `.note` 笔记请在客户端编辑。

**Q: 创建 Markdown 笔记的正确方式**
使用 `save` 命令并指定 `"type": "md"`，详见 [笔记管理示例](examples/note-management.md)。

**Q: API Key 在哪里获取**
登录 [mopen.163.com](https://mopen.163.com/#/dashboard)，进入「API 管理」获取。

---

## English

### Introduction

This project provides two [Claude Code](https://claude.ai/code) Skills for YoudaoNote (有道云笔记), allowing you to manage notes and clip web pages using natural language directly in Claude Code.

| Skill | Features |
|-------|----------|
| `youdaonote-notes` | List / read / create / update / delete / search notes, manage todos |
| `youdaonote-clip` | Web clipping, view recent collections |

### AI Agent Setup Guide

> This section is designed for AI agents. Follow the steps in order to complete the full setup.

#### Step 1: Check and install youdaonote CLI

```bash
which youdaonote || ~/.local/bin/youdaonote --version
```

- **If found** → skip to Step 2
- **If not found** → install:

```bash
curl -fsSL https://artifact.lx.netease.com/download/youdaonote-cli/install.sh | bash
```

Verify using the full path (PATH not yet refreshed in current shell):

```bash
~/.local/bin/youdaonote --version
```

**Success indicator:** version number printed, e.g. `1.2.0`

---

#### Step 2: Configure API Key

**Before this step, ask the user:**

> Please visit https://mopen.163.com/#/dashboard to get your YoudaoNote API Key (login required, phone number must be linked). Share the key with me when ready.

Once the user provides the key:

```bash
~/.local/bin/youdaonote config set apiKey <USER_PROVIDED_KEY>
```

**Success indicator:** output says `已写入 mcp.apiKey`

---

#### Step 3: Verify connection

```bash
~/.local/bin/youdaonote check
```

**Success indicator:** all items show `✅`, last line reads `✅ 所有检查通过`

- `❌ api-key` → wrong key, go back to Step 2
- `❌ mcp-connection` → network issue, ask user to check connectivity

---

#### Step 4: Install Skill files

```bash
mkdir -p ~/.claude/skills/youdaonote-notes ~/.claude/skills/youdaonote-clip

curl -fsSL https://raw.githubusercontent.com/HViktorTsoi/youdaonote-claude-skills/main/skills/youdaonote-notes/SKILL.md \
  -o ~/.claude/skills/youdaonote-notes/SKILL.md

curl -fsSL https://raw.githubusercontent.com/HViktorTsoi/youdaonote-claude-skills/main/skills/youdaonote-clip/SKILL.md \
  -o ~/.claude/skills/youdaonote-clip/SKILL.md
```

Verify:

```bash
ls ~/.claude/skills/youdaonote-notes/SKILL.md ~/.claude/skills/youdaonote-clip/SKILL.md
```

**Success indicator:** both files listed without error.

---

#### Step 5: Confirm completion

Inform the user:

> Setup complete! Please **restart Claude Code**. You can then manage your YoudaoNote by chatting naturally, e.g.:
> - "List my YoudaoNote notes"
> - "Clip this article: https://..."

---

### Manual Installation

```bash
# 1. Install CLI
curl -fsSL https://artifact.lx.netease.com/download/youdaonote-cli/install.sh | bash

# 2. Set API Key
youdaonote config set apiKey YOUR_API_KEY
youdaonote check

# 3. Install Skills
mkdir -p ~/.claude/skills/youdaonote-notes ~/.claude/skills/youdaonote-clip

curl -fsSL https://raw.githubusercontent.com/HViktorTsoi/youdaonote-claude-skills/main/skills/youdaonote-notes/SKILL.md \
  -o ~/.claude/skills/youdaonote-notes/SKILL.md

curl -fsSL https://raw.githubusercontent.com/HViktorTsoi/youdaonote-claude-skills/main/skills/youdaonote-clip/SKILL.md \
  -o ~/.claude/skills/youdaonote-clip/SKILL.md
```

Restart Claude Code — Skills activate automatically.

### Usage Examples

```
List my YoudaoNote notes
Search my notes for "machine learning"
Create a new note titled "Meeting Notes" in the Work folder
Clip this article to YoudaoNote: https://example.com/article
Show my recent 20 clippings
List all my todos
```

### Contributing

Issues and PRs are welcome! If you find a bug or want to improve the Skills, feel free to open an issue.

### License

[MIT](LICENSE) © HViktorTsoi
