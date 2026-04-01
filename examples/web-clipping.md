# 网页剪藏示例 | Web Clipping Examples

## 剪藏网页

**基本剪藏**
```
你：帮我剪藏这篇文章 https://arxiv.org/abs/2404.12345
Claude：youdaonote clip "https://arxiv.org/abs/2404.12345"
✅ 已剪藏「PointNeRF++: A multi-scale, point-based ...」到有道云笔记
```

**剪藏到指定目录**
```
你：把这篇博客存到"学习"目录
Claude：（先获取目录 ID，再剪藏）
youdaonote clip "https://example.com/blog" -f <folderID>
✅ 已保存到「学习」
```

**URL 包含特殊字符**
```
你：剪藏 https://example.com/page?id=1&type=article
Claude：（URL 含 & 字符，自动加引号处理）
youdaonote clip "https://example.com/page?id=1&type=article"
✅ 已剪藏
```

---

## 查看最近收藏

```
你：查看我最近收藏的文章
Claude：youdaonote recent
1. PointNeRF++ - 2026-04-01
2. Attention Is All You Need - 2026-03-29
3. ...（共 15 条）

你：显示最近 30 条收藏
Claude：youdaonote recent -l 30
```

---

## 常见场景

**论文 / 技术文章收藏**
```
你：把这篇 CVPR 论文页面存到笔记 https://openaccess.thecvf.com/...
```

**开发文档收藏**
```
你：把 PyTorch 的这页文档保存到有道云 https://pytorch.org/docs/...
```

**剪藏失败时**
```
你：剪藏失败了怎么办？
Claude：可以增大超时时间后重试：
export YOUDAONOTE_MCP_TIMEOUT=120
youdaonote clip "URL"
```
