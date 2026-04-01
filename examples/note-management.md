# 笔记管理示例 | Note Management Examples

## 浏览笔记

**列出根目录**
```
你：帮我列出有道云笔记的根目录
Claude：
📁 日常记录
📁 工作
📁 学习
📁 任务清单
📄 示例笔记.note
```

**进入子目录**
```
你：列出"工作"目录里的笔记
Claude：先获取目录 ID，再列出内容：
📁 项目A
📁 项目B
📄 想法.note
📄 文章摘录.note
```

**搜索笔记**
```
你：搜索我笔记里关于"机器学习"的内容
Claude：youdaonote search "机器学习"
找到 3 条结果：
📄 文章摘录A.note
📄 学习笔记.note
📄 想法.note
```

---

## 读取笔记

```
你：读取"想法.note"的内容
Claude：（先 list 获取 fileId，再 read）
# 工作想法
- 规划方向一
- ...
```

---

## 创建笔记

### 创建富文本笔记（.note 格式）

```
你：在"归档"目录下创建一篇今日会议记录
Claude：youdaonote create -n "2026-04-01 会议记录" -c "..." -f <folderID>
✅ 已创建笔记：2026-04-01 会议记录
```

### 创建 Markdown 笔记（.md 格式）

> `create` 命令只能创建富文本笔记，Markdown 笔记需使用 `save` 命令。

```
你：在"工作"目录下新建一篇 Markdown 格式的笔记
Claude：
1. 生成 JSON payload（Python）
2. 调用 youdaonote save --file /tmp/note_payload.json
3. 若目录未生效则 move 到目标目录
✅ 已创建 Markdown 笔记：工作笔记.md
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
[ ] 完成项目报告  截止：2026-04-10
[ ] 准备演讲材料  截止：2026-04-03
[x] 代码审查      已完成

你：新建一个待办：准备季度总结，截止 4 月 20 日
Claude：youdaonote todo create -t "准备季度总结" -d 2026-04-20
✅ 已创建

你：把"完成项目报告"标记为完成
Claude：youdaonote todo update <todoId> --done
✅ 已完成
```
