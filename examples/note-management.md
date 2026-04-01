# 笔记管理示例 | Note Management Examples

## 浏览笔记

**列出根目录**
```
你：帮我列出有道云笔记的根目录
Claude：
📁 会议纪要
📁 0# Research
📁 1# 论文&代码阅读笔记
📁 TODO
📄 高科技.note
```

**进入子目录**
```
你：列出"0# Research"目录里的笔记
Claude：先获取目录 ID，再列出内容：
📁 VI-SLAM
📁 Actuated LiDAR
📄 IDEAs.note
📄 Paper Snippets.note
```

**搜索笔记**
```
你：搜索我笔记里关于"点云分割"的内容
Claude：youdaonote search "点云分割"
找到 3 条结果：
📄 5篇CVPR 点云分割相关...
📄 pc2img.note
📄 IDEAs.note
```

---

## 读取笔记

```
你：读取"IDEAs.note"的内容
Claude：（先 list 获取 fileId，再 read）
# Research Ideas
- 基于时序一致性的动态点云分割
- ...
```

---

## 创建笔记

### 创建富文本笔记（.note 格式）

```
你：在 Legacy 目录下创建一篇今日会议记录
Claude：youdaonote create -n "2026-04-01 会议记录" -c "..." -f WEB65d95b...
✅ 已创建笔记：2026-04-01 会议记录
```

### 创建 Markdown 笔记（.md 格式）

> `create` 命令只能创建富文本笔记，Markdown 笔记需使用 `save` 命令。

```
你：在 Research 目录下新建一篇 Markdown 格式的论文笔记
Claude：
1. 生成 JSON payload（Python）
2. 调用 youdaonote save --file /tmp/note_payload.json
3. 若目录未生效则 move 到目标目录
✅ 已创建 Markdown 笔记：论文阅读笔记.md
```

---

## 更新 / 删除笔记

```
你：把刚才的笔记内容改成"更新后的内容"
Claude：youdaonote update <fileId> -c "更新后的内容"
（注：仅支持 .md 格式笔记）

你：删除那篇测试笔记
Claude：youdaonote delete <fileId>
✅ 已删除
```

---

## 待办事项管理

```
你：列出我所有的待办事项
Claude：youdaonote todo list
[ ] 提交论文初稿  截止：2026-04-10
[ ] 组会 PPT     截止：2026-04-03
[x] 代码 review  已完成

你：新建一个待办：准备答辩材料，截止 4 月 20 日
Claude：youdaonote todo create -t "准备答辩材料" -d 2026-04-20
✅ 已创建

你：把"提交论文初稿"标记为完成
Claude：youdaonote todo update <todoId> --done
✅ 已完成
```
