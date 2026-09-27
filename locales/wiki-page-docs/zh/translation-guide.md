---
title: 国际化 (i18n) 指南
description: 在 FloWorks 中添加和管理翻译的分步说明
---

# 🌐 国际化 (i18n) 指南

本文档介绍如何向 FloWorks 添加新语言并有效管理翻译文件。

---

## ➕ 如何添加新语言

### 步骤 1：创建 JSON 文件
导航到 `locales/` 文件夹。复制 `en.json` 并使用相应的双字母 [ISO 639-1](https://zh.wikipedia.org/wiki/ISO_639-1) 代码重命名（例如，法语为 `fr.json`，德语为 `de.json`）。

### 步骤 2：翻译字符串
在文本编辑器中打开新的 JSON 文件。

!!! warning "不要修改键"
    **永远不要更改键**（每对中的左侧）。只需翻译值（右侧）。

**原始 (`en.json`)：**
```json
{
  "app_title": "FloWorks",
  "menu": {
    "file": "File",
    "edit": "Edit"
  }
}
```

**翻译示例 (`es.json`)：**
```json
{
  "app_title": "FloWorks",
  "menu": {
    "file": "Archivo",
    "edit": "Editar"
  }
}
```
确保根键 `"language_name"` 包含该语言的本地名称（例如 `"Français"`、`"Deutsch"`、`"Español"`）。

### 步骤 3：验证 JSON
验证文件是否为有效的 JSON（没有尾随逗号、正确的引号、适当的转义）。您可以使用在线验证器，例如 [JSONLint](https://jsonlint.com)，或运行：
```bash
python -m json.tool locales/es.json
```

### 步骤 4：测试新语言
1. 启动 FloWorks。
2. 转到 **INFO → 语言** 并选择新语言。
3. 验证所有界面元素是否立即更新（菜单、面板、对话框、节点标签等）。

### 步骤 5：自动检测（可选）
如果用户的系统区域设置与新语言代码匹配，FloWorks 将在首次启动时自动使用它（只要在 `QSettings` 中未保存先前的首选项）。

---

## 🌍 可用语言
- **英语** (`en`) – 基础 / 回退语言
- **西班牙语** (`es`)

---

## ⚙️ 重要说明和良好实践

!!! info "回退机制"
    基础语言是**英语**。如果语言文件中缺少某个翻译键，FloWorks 会自动使用英语字符串作为回退。

!!! warning "防止 UI 溢出"
    保持翻译简洁，以避免布局破坏。如果翻译后的文本明显更长，请考虑缩写或依靠主题系统来处理动态缩放。

!!! tip "保留 HTML 和占位符"
    - **HTML 标签：** 保持所有 HTML 标签完全不变（例如 `<h3>`、`<b>`、`<pre>`、`<br>`）。
    - **占位符：** 在使用 `{variable}` 语法的任何地方保持该语法（例如 `"语言已更改为：{name} ({code})"`）。不要重新排序或删除它们。

---

## 🔗 相关文档
- [📖 代码地图和架构](architecture-ii.md)
- [📦 构建和分发指南](build.md)
- [🧩 节点参考](node-reference.md)
