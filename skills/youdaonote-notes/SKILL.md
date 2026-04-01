---
name: youdaonote-notes
description: 当用户想要"查看有道云笔记"、"列出笔记"、"创建笔记"、"搜索笔记"、"读取笔记内容"、"更新笔记"、"删除笔记"、"移动笔记"、"重命名笔记"、"管理待办事项"、"todo"、"有道笔记"相关操作时，使用此 skill。
version: 1.0.0
---

# 有道云笔记 · 笔记管理 Skill

## 前置条件

```bash
# 安装 CLI（macOS/Linux）
curl -fsSL https://artifact.lx.netease.com/download/youdaonote-cli/install.sh | bash

# 配置 API Key（从 https://mopen.163.com/#/dashboard 获取）
youdaonote config set apiKey YOUR_API_KEY
```

## 安装信息

| 项目 | 路径 |
|------|------|
| CLI 二进制 | `~/.local/bin/youdaonote` |
| 配置文件 | `~/.youdaonote.json` |
| PATH 配置 | 安装脚本自动写入 `~/.zshrc`，新终端生效 |

> 若当前终端尚未 source `~/.zshrc`，请用完整路径调用：`~/.local/bin/youdaonote <command>`

---

## 笔记操作命令

### 查看笔记列表

```bash
# 列出根目录笔记
youdaonote list

# 列出指定目录的笔记（需目录 ID）
youdaonote list -f <folderID>
```

> ⚠️ `list` 不支持 `--json` 参数，输出为纯文本格式。

### 读取笔记内容

```bash
youdaonote read <fileId>
```

### 创建笔记（富文本 .note 格式）

> ⚠️ `create` 命令**固定创建 `.note` 富文本格式**，无法创建 Markdown 笔记。

```bash
# 创建带标题和内容的笔记
youdaonote create -n "笔记标题" -c "笔记内容"

# 只创建标题（内容为空）
youdaonote create -n "笔记标题"

# 内容超过 10KB 时，从文件读取
youdaonote create -n "笔记标题" --file content.md

# 保存到指定目录
youdaonote create -n "笔记标题" -c "内容" -f <folderID>
```

### 创建 Markdown 笔记（.md 格式）

`create` 命令无法创建 `.md` 笔记，必须使用 `save` 命令并指定 `"type": "md"`。

**步骤一：** 用 Python 生成 JSON 文件（避免换行符转义问题）

```python
import json

with open('/tmp/note_content.md', 'r') as f:
    content = f.read()

payload = {
    "title": "笔记标题",   # 不需要带 .md 后缀，save 命令会自动处理
    "type": "md",          # 关键：指定 md 类型
    "content": content,
    "parentId": "<folderID>"  # 目标目录 ID；不传则存到「我的资源」
}

with open('/tmp/note_payload.json', 'w') as f:
    json.dump(payload, f, ensure_ascii=False)
```

**步骤二：** 调用 save 命令

```bash
youdaonote save --file /tmp/note_payload.json
# 输出：笔记 ID：XXXXXXXX
```

**步骤三：** 若 parentId 不生效，手动移动

```bash
# save 命令有时忽略 parentId，笔记会落在默认目录
# 用 search 找到笔记后再 move
youdaonote search "笔记标题"
youdaonote move <fileId> <targetFolderID>
```

> `save` 命令支持的 `type` 值：`note`（富文本）、`md`（Markdown）、`mindmap2`（思维导图）

### 更新笔记

```bash
# 更新笔记内容（仅支持 Markdown 格式笔记）
youdaonote update <fileId> -c "新内容"

# 从文件更新
youdaonote update <fileId> --file new_content.md
```

### 删除笔记

```bash
# 删除笔记（不能删除文件夹）
youdaonote delete <fileId>
```

### 重命名笔记

```bash
youdaonote rename <fileId> "新标题"
```

### 移动笔记

```bash
youdaonote move <fileId> <targetFolderID>
```

### 搜索笔记

```bash
youdaonote search "关键词"
```

---

## 待办事项（Todo）操作

### 查看任务

```bash
# 查看所有任务
youdaonote todo list

# 查看指定分组任务
youdaonote todo list -g <groupID>

# JSON 格式
youdaonote todo list --json
```

### 创建任务

```bash
# 基本创建
youdaonote todo create -t "任务标题"

# 带截止日期（格式：YYYY-MM-DD）
youdaonote todo create -t "任务标题" -d 2026-04-30

# 创建到指定分组
youdaonote todo create -t "任务标题" -g <groupID>
```

### 更新任务状态

```bash
# 标记为完成
youdaonote todo update <todoId> --done

# 标记为未完成
youdaonote todo update <todoId> --undone

# 修改标题
youdaonote todo update <todoId> -t "新标题"
```

### 管理任务分组

```bash
# 列出所有分组
youdaonote todo groups

# 创建分组
youdaonote todo group-create "分组名称"
```

---

## 工作流程指南

1. **用户要列出笔记** → `youdaonote list`，如需进入子目录先获取目录 ID
2. **用户要搜索笔记** → `youdaonote search "关键词"`
3. **用户要创建富文本笔记** → `youdaonote create -n "标题" -c "内容"`，内容长时用 `--file`
4. **用户要创建 Markdown 笔记** → 用 Python 生成 JSON（含 `"type": "md"`），再调用 `youdaonote save --file payload.json`，最后用 `move` 移至目标目录
5. **用户要读取笔记** → 先 `list` 获取 fileId，再 `youdaonote read <fileId>`
6. **用户要管理 Todo** → 使用 `youdaonote todo` 系列命令

## 错误处理

- `API Key 未配置`：引导用户运行 `youdaonote config set apiKey YOUR_API_KEY`
- `fileId 未知`：先执行 `list` 或 `search` 找到对应 ID
- `update 失败`：确认目标笔记为 Markdown 格式（不支持富文本笔记更新）
- `命令未找到`：先尝试完整路径 `~/.local/bin/youdaonote`，若仍失败则重新安装 CLI
