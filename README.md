# YoudaoNote Claude Code Skills

**有道云笔记 × Claude Code —— 用自然语言操作你的笔记**

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Claude Code](https://img.shields.io/badge/Claude%20Code-Skill-blue)](https://claude.ai/code)
[![Platform](https://img.shields.io/badge/Platform-macOS%20%7C%20Linux-lightgrey)](https://note.youdao.com)

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
你：帮我列出 0# Research 目录里的笔记
Claude：📁 VI-SLAM  📄 IDEAs.note  📄 Paper Snippets.note ...

你：把这篇文章保存到有道云笔记 https://arxiv.org/abs/2404.xxxxx
Claude：✅ 已剪藏「XXX: A Survey」到有道云笔记

你：在 Legacy 目录下创建一篇今日会议记录
Claude：✅ 已创建笔记「2026-04-01 会议记录」
```

### 安装

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

将 skills 目录复制到 Claude Code 全局 skills 目录：

```bash
# 克隆本仓库
git clone https://github.com/HViktorTsoi/youdaonote-claude-skills.git
cd youdaonote-claude-skills

# 安装到全局 skills 目录
cp -r skills/youdaonote-notes ~/.claude/skills/
cp -r skills/youdaonote-clip ~/.claude/skills/
```

安装完成后，重启 Claude Code，Skills 即自动生效。

### 使用示例

#### 笔记管理

```
列出我的有道云笔记
帮我搜索笔记里关于"深度学习"的内容
在 Research 目录下新建一篇笔记，标题是"论文阅读笔记"
读取笔记 <fileId> 的内容
把笔记重命名为"新标题"
```

#### 网页剪藏

```
帮我剪藏这篇文章 https://example.com/article
查看我最近收藏的 20 篇文章
把这个链接保存到有道云笔记的 Research 目录
```

#### 待办事项

```
列出我的所有待办事项
新建一个待办：明天提交论文初稿，截止日期 2026-04-02
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

### Installation

#### Step 1: Install YoudaoNote CLI

```bash
curl -fsSL https://artifact.lx.netease.com/download/youdaonote-cli/install.sh | bash
```

#### Step 2: Configure API Key

Get your API Key from the [NetEase Developer Platform](https://mopen.163.com/#/dashboard) (requires a YoudaoNote account with a linked phone number):

```bash
youdaonote config set apiKey YOUR_API_KEY
youdaonote check   # verify connection
```

#### Step 3: Install Skills

```bash
git clone https://github.com/HViktorTsoi/youdaonote-claude-skills.git
cd youdaonote-claude-skills

cp -r skills/youdaonote-notes ~/.claude/skills/
cp -r skills/youdaonote-clip ~/.claude/skills/
```

Restart Claude Code — the Skills will activate automatically.

### Usage Examples

```
List my YoudaoNote notes
Search my notes for "machine learning"
Create a new note titled "Meeting Notes" in the Research folder
Clip this article to YoudaoNote: https://example.com/article
Show my recent 20 clippings
List all my todos
```

### Contributing

Issues and PRs are welcome! If you find a bug or want to improve the Skills, feel free to open an issue.

### License

[MIT](LICENSE) © HViktorTsoi
