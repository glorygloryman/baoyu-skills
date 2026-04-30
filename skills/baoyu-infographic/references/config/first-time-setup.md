---
name: first-time-setup
description: First-time setup flow for baoyu-infographic preferences
---

# First-Time Setup

## Overview

When no EXTEND.md is found, guide the user through preference setup before generating any infographic. Saved preferences shift Step-3 recommendations and Step-4 defaults only — they never bypass Step 4 confirmation (see the `## Confirmation Policy` section in SKILL.md).

**⛔ BLOCKING OPERATION**: This setup MUST complete before ANY other workflow steps. Do NOT:
- Ask about source content or topic
- Ask about layout, style, or aspect
- Begin Step 1.2 content analysis

ONLY ask the questions in this setup flow, save EXTEND.md, then continue to Step 1.2.

## Setup Flow

```
No EXTEND.md found
        │
        ▼
┌─────────────────────┐
│ AskUserQuestion     │
│ (all questions)     │
└─────────────────────┘
        │
        ▼
┌─────────────────────┐
│ Create EXTEND.md    │
└─────────────────────┘
        │
        ▼
    Continue to Step 1.2
```

## Questions

**Language**: 使用用户输入的语言展示问题文本（中文用户用中文问，英文用户用英文问）。**请勿固定使用英文**。下面给出中文模板，英文用户请按对应字段直译。

每个选项必须附带"适用描述"（description 字段），说明该选项最适合哪类汇报场景，参考 SKILL.md 的 `Recommended Combinations` 表。

Use a single `AskUserQuestion` with multiple questions (the runtime auto-adds an "Other" option):

### Question 1: 默认版式（Preferred Layout）

```
header: "默认版式"
question: "请选择默认的信息图版式？"
options:
  - label: "自动选择（推荐）"
    description: "Step 3 根据内容类型自动推荐最合适的版式"
  - label: "bento-grid"
    description: "多主题概览 — 周报 / 月报 / 项目情况汇报"
  - label: "structural-breakdown"
    description: "爆炸拆解图 — 代码 / 系统架构讲解"
  - label: "dashboard"
    description: "指标看板 — 项目状态 / KPI / 风险红绿灯"
  - label: "dense-modules"
    description: "高密度模块 — 深度技术普及长图"
```

### Question 2: 默认风格（Preferred Style）

```
header: "默认风格"
question: "请选择默认的视觉风格？"
options:
  - label: "自动选择（推荐）"
    description: "Step 3 根据内容调性自动推荐最合适的风格"
  - label: "corporate-memphis"
    description: "扁平矢量、明亮 — 向管理层汇报最稳（默认）"
  - label: "technical-schematic"
    description: "工程蓝图 — 代码 / 架构讲解王者"
  - label: "pop-laboratory"
    description: "蓝图坐标网格、实验室精度 — 工程师审美最高公约数"
  - label: "ikea-manual"
    description: "极简线稿 — SOP / 规范 / 操作手册首选"
```

### Question 3: 默认比例（Preferred Aspect）

```
header: "默认比例"
question: "请选择默认的画幅比例？"
options:
  - label: "自动选择（推荐）"
    description: "Step 4 根据版式自动推荐合适的比例"
  - label: "landscape 16:9"
    description: "横版 — 会议演示 / PPT 嵌入 / 飞书文档（推荐）"
  - label: "portrait 9:16"
    description: "竖版 — 制度类 / 深度普及 / 手机端阅读 / 群内传播"
  - label: "square 1:1"
    description: "方版 — 社交贴文 / 缩略图"
```

### Question 4: 信息图文字语言（Output Language）

```
header: "信息图文字语言"
question: "信息图中文字使用什么语言？"
options:
  - label: "自动检测（推荐）"
    description: "与源内容语言保持一致"
  - label: "zh 中文"
    description: "强制使用中文输出"
  - label: "en 英文"
    description: "强制使用英文输出"
```

### Question 5: 偏好保存位置（Save Location）

```
header: "偏好保存位置"
question: "偏好配置保存到哪里？"
options:
  - label: "Project：只对当前项目生效"
    description: "保存到 .baoyu-skills/baoyu-infographic/EXTEND.md"
  - label: "User：对所有项目生效"
    description: "保存到 ~/.baoyu-skills/baoyu-infographic/EXTEND.md（推荐）"
```

## Save Locations

| Choice | Path | Scope |
|--------|------|-------|
| Project | `.baoyu-skills/baoyu-infographic/EXTEND.md` | Current project |
| User | `~/.baoyu-skills/baoyu-infographic/EXTEND.md` | All projects |

XDG path (`${XDG_CONFIG_HOME:-$HOME/.config}/baoyu-skills/baoyu-infographic/EXTEND.md`) is also recognized at read time but not offered as a save target during first-time setup.

## After Setup

1. Create the directory if needed
2. Write EXTEND.md with frontmatter (see template below)
3. Confirm: "Preferences saved to [path]"
4. Continue to Step 1.2

## EXTEND.md Template

```yaml
---
version: 1
preferred_layout: [selected layout or null]
preferred_style: [selected style or null]
preferred_aspect: [landscape|portrait|square|null]
language: [selected language or null]
preferred_image_backend: auto
image_model: gpt-image-2
custom_styles: []
---
```

`preferred_image_backend: auto` 是内置默认值——首次设置不询问。SKILL.md 中的 `## Image Generation Tools` 规则会优先选择运行时原生工具（Codex `imagegen`、Hermes `image_generate` 等），无原生工具时回落到已安装后端（如 `baoyu-imagine`）。

`image_model: gpt-image-2` 是内置默认值——首次设置同样不询问。当后端为 Codex `imagegen` 或其它支持显式模型选择的图像 API 时，Step 6 必须把该字段传给后端的 `model` 参数；如后端不接受该参数则忽略，但**不要静默降级到其它模型**。如需改用其它模型，编辑 EXTEND.md 即可。

## Modifying Preferences Later

See the `## Changing Preferences` section in `SKILL.md` for the canonical list of common edits (pin backend, change layout/style defaults, retrigger setup). Full schema: `references/config/preferences-schema.md`.
