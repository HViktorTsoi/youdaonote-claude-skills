---
name: youdaonote-clip
description: 当用户想要"剪藏网页"、"保存文章到有道云笔记"、"clip 链接"、"收藏网页"、"把这个网址存到笔记"、"查看最近收藏"、"recent 笔记"时，使用此 skill。
version: 1.0.0
---

# 有道云笔记 · 网页剪藏 Skill

## 前置条件

使用前确认 CLI 已安装并配置好 API Key：

```bash
# 安装 CLI（macOS/Linux）
curl -fsSL https://artifact.lx.netease.com/download/youdaonote-cli/install.sh | bash

# 配置 API Key（从 https://mopen.163.com/#/dashboard 获取）
youdaonote config set apiKey YOUR_API_KEY

# 验证
youdaonote recent
```

## 安装信息

| 项目 | 路径 |
|------|------|
| CLI 二进制 | `~/.local/bin/youdaonote` |
| 配置文件 | `~/.youdaonote.json` |
| PATH 配置 | 安装脚本自动写入 `~/.zshrc`，新终端生效 |

> 若当前终端尚未 source `~/.zshrc`，请用完整路径调用：`~/.local/bin/youdaonote <command>`

## 主要操作

### 剪藏网页

```bash
# 基本剪藏
youdaonote clip "https://example.com/article"

# 剪藏到指定目录（需要目录 ID）
youdaonote clip "https://example.com/article" -f <folderID>

# 保存剪藏原始 HTML 数据到本地文件
youdaonote clip-save --file result.json
```

### 查看最近收藏

```bash
# 查看最近 15 条收藏
youdaonote recent

# 查看最近 N 条收藏
youdaonote recent -l 30

# 以 JSON 格式输出
youdaonote recent -c
```

## 工作流程

1. 用户提供网址 → 运行 `youdaonote clip "URL"` 执行剪藏
2. 用户要查看收藏 → 运行 `youdaonote recent` 列出最近收藏
3. 如剪藏失败（网络超时等）→ 告知用户可设置环境变量 `YOUDAONOTE_MCP_TIMEOUT=120` 增大超时时间
4. 如遇到 URL 含 `&` 字符 → 提醒用户用引号包裹 URL

## 错误处理

- `API Key 未配置`：引导用户运行 `youdaonote config set apiKey YOUR_API_KEY`
- `命令未找到`：先尝试完整路径 `~/.local/bin/youdaonote`，若仍失败则重新安装 CLI
- `剪藏失败`：检查网络、确认 URL 可访问、尝试增大超时

## 环境变量

| 变量 | 说明 | 示例 |
|------|------|------|
| `YOUDAONOTE_API_KEY` | API 密钥（优先级高于 config） | `export YOUDAONOTE_API_KEY="xxx"` |
| `YOUDAONOTE_MCP_TIMEOUT` | 请求超时秒数（默认 60） | `export YOUDAONOTE_MCP_TIMEOUT=120` |
