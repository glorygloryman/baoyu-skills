# baoyu-infographic 使用手册（技术负责人定制版）

> 适用对象：IT 公司技术负责人，日常汇报内容包括管理制度、项目情况、代码讲解、技术普及。
> 创建日期：2026-04-30
> 用途：在 Codex / Claude Code 等运行时安装 `baoyu-infographic` skill 后，作为个人速查与默认配置参考。

---

## 1. 核心理念

**信息图 = 布局（Layout）× 风格（Style）的自由组合。**

- 21 种布局：决定"信息怎么组织"
- 21 种视觉风格：决定"画面长什么样"
- 21 × 21 = 441 种组合，按内容类型选最合适的搭配

工作流（7 步）：分析内容 → 结构化 → 推荐组合 → **确认门（强制）** → 生成 prompt → 调后端出图 → 汇报结果。除非显式 `--no-confirm`，否则必须确认。

---

## 2. 21 种布局清单（标注本人适配度）

| 布局 | 适用 | 适配度 |
|---|---|---|
| `linear-progression` | 时间线、流程、教程 | ⭐⭐⭐ 制度流程 / 项目里程碑 |
| `binary-comparison` | A vs B、前后对比 | ⭐⭐⭐ 技术选型、重构前后 |
| `comparison-matrix` | 多维对比 | ⭐⭐⭐ 技术栈选型矩阵 |
| `hierarchical-layers` | 金字塔、优先级 | ⭐⭐ 管理层级、技术分级 |
| `tree-branching` | 分类、分类法 | ⭐⭐ 技术体系树 |
| `hub-spoke` | 中心 + 关联 | ⭐⭐⭐ 技术普及、概念讲解 |
| `structural-breakdown` | 爆炸图、剖面 | ⭐⭐⭐⭐ **代码 / 系统架构拆解** |
| `bento-grid` | 多主题概览（默认） | ⭐⭐⭐⭐ **周报 / 月报最佳容器** |
| `iceberg` | 表面 vs 隐藏 | ⭐⭐ 技术债、隐性成本 |
| `bridge` | 问题→解决方案 | ⭐⭐⭐ 技术方案汇报 |
| `funnel` | 漏斗、过滤 | ⭐⭐ 招聘漏斗、需求过滤 |
| `isometric-map` | 等距空间关系 | ⭐⭐⭐ 系统部署拓扑 |
| `dashboard` | 指标、KPI | ⭐⭐⭐⭐ **项目状态汇报标配** |
| `periodic-table` | 分类集合 | ⭐⭐ 工具集 / API 全景 |
| `comic-strip` | 叙事、序列 | ⭐ 偶尔用于事故复盘 |
| `story-mountain` | 情节弧线 | — 不适合 |
| `jigsaw` | 互联模块 | ⭐⭐ 微服务关系 |
| `venn-diagram` | 重叠概念 | ⭐⭐ 职责边界、技能交集 |
| `winding-roadmap` | 旅程、里程碑 | ⭐⭐⭐ 季度路线图 |
| `circular-flow` | 周期循环 | ⭐⭐⭐ DevOps / CI-CD 闭环 |
| `dense-modules` | 高密度模块 | ⭐⭐⭐⭐ **深度技术普及长图** |

---

## 3. 21 种风格清单（标注本人适配度）

| 风格 | 描述 | 适配度 |
|---|---|---|
| `craft-handmade` | 手绘纸艺（默认） | ⭐⭐ 内部培训可用 |
| `claymation` | 3D 黏土定格 | ❌ 不适合技术汇报 |
| `kawaii` | 日系可爱 | ❌ 不适合技术汇报 |
| `storybook-watercolor` | 柔和水彩童话 | ❌ 偏儿童 |
| `chalkboard` | 黑板粉笔 | ⭐⭐⭐ **技术分享 / 培训氛围** |
| `cyberpunk-neon` | 霓虹未来 | ⭐⭐ 大会海报 / 发布会 |
| `bold-graphic` | 漫画粗线、半调 | ⭐⭐ 对外科普文章 |
| `aged-academia` | 复古科学、做旧 | ⭐⭐⭐ 深度原理、学术感 |
| `corporate-memphis` | 扁平矢量、明亮 | ⭐⭐⭐⭐ **对管理层最稳** |
| `technical-schematic` | 工程蓝图 | ⭐⭐⭐⭐ **代码 / 架构讲解王者** |
| `origami` | 折纸几何 | ❌ 装饰性大于信息性 |
| `pixel-art` | 8-bit 像素 | ❌ 不严肃 |
| `ui-wireframe` | 灰阶线框 | ⭐⭐⭐⭐ **系统设计 / 技术评审** |
| `subway-map` | 地铁线路图 | ⭐⭐⭐ 系统数据流、调用链 |
| `ikea-manual` | 极简线稿 | ⭐⭐⭐⭐ **SOP / 规范首选** |
| `knolling` | 工具有序俯视 | ⭐⭐⭐ 技术栈 / 工具盘点 |
| `lego-brick` | 乐高积木 | ❌ 太玩具感 |
| `pop-laboratory` | 蓝图坐标网格 | ⭐⭐⭐⭐ **工程师审美最高公约数** |
| `morandi-journal` | 莫兰迪手绘笔记 | ⭐⭐ 内部学习笔记 |
| `retro-pop-grid` | 70 年代瑞士网格 | ⭐⭐ 对外品牌内容 |
| `hand-drawn-edu` | 马卡龙手绘、火柴人 | ⭐⭐ 新人培训 |

---

## 4. 四类汇报场景的强烈推荐组合

### 4.1 管理制度（流程规范、研发规约、Code Review 守则、值班制度）

| 推荐度 | 组合 | 理由 |
|---|---|---|
| 🥇 首选 | `linear-progression` + `ikea-manual` | 步骤清晰、极简线稿，像 IKEA 说明书一样不容误解 |
| 🥈 次选 | `hierarchical-layers` + `corporate-memphis` | 等级 / 优先级一目了然，对管理层友好 |
| 🥉 备选 | `bento-grid` + `corporate-memphis` | 多条规约一图汇总 |

### 4.2 项目情况（周报、月报、里程碑、风险红绿灯）

| 推荐度 | 组合 | 理由 |
|---|---|---|
| 🥇 首选 | `dashboard` + `corporate-memphis` | KPI 看板的天然形态，老板一眼看懂 |
| 🥈 次选 | `winding-roadmap` + `corporate-memphis` | 季度 / 半年路线图，里程碑视觉化 |
| 🥉 备选 | `bento-grid` + `pop-laboratory` | 多项目并列概览，工程师审美 |

### 4.3 代码讲解（架构图、模块拆解、调用链、重构前后）

| 推荐度 | 组合 | 理由 |
|---|---|---|
| 🥇 首选 | `structural-breakdown` + `technical-schematic` | 爆炸拆解 + 工程蓝图 = 阅读架构最佳载体 |
| 🥈 次选 | `subway-map` + `technical-schematic` | 适合表达数据流、调用链、消息流转 |
| 🥉 备选 | `binary-comparison` + `ui-wireframe` | 重构前 vs 后、新旧方案对比 |
| 加分 | `isometric-map` + `pop-laboratory` | 微服务部署拓扑、立体感强 |

### 4.4 技术普及（团队分享、新员工培训、对外科普）

| 推荐度 | 组合 | 理由 |
|---|---|---|
| 🥇 首选 | `hub-spoke` + `hand-drawn-edu` | 一图讲清一个概念，最适合科普 |
| 🥈 次选 | `dense-modules` + `pop-laboratory` | 高密度技术长图，公众号 / 内部 wiki 直接发 |
| 🥉 备选 | `bento-grid` + `chalkboard` | 课堂氛围，适合内部分享会预热 |
| 对外加分 | `dense-modules` + `retro-pop-grid` | 流行视觉，对外传播打开率高 |

---

## 5. 推荐的 EXTEND.md 默认配置

安装 skill 后，把以下内容写入：

**路径优先级**（按顺序，第一个找到即生效）：
1. 项目级：`<project>/.baoyu-skills/baoyu-infographic/EXTEND.md`
2. XDG：`${XDG_CONFIG_HOME:-$HOME/.config}/baoyu-skills/baoyu-infographic/EXTEND.md`
3. 用户级：`$HOME/.baoyu-skills/baoyu-infographic/EXTEND.md`

**推荐内容**：

```yaml
# 默认布局：万金油，适合 80% 的汇报场景
preferred_layout: bento-grid

# 默认风格：管理层不会出错的扁平专业风
preferred_style: corporate-memphis

# 默认横版：方便会议演示 / PPT 嵌入 / 飞书文档
preferred_aspect: landscape

# 默认中文
language: zh

# 图像后端自动选择（优先 Codex 内置 imagegen，否则 baoyu-imagine）
preferred_image_backend: auto
```

---

## 6. 三种场景的手动覆盖命令

固定默认后，只在下面 3 类场景手动指定：

```bash
# 1. 讲架构 / 代码
--layout structural-breakdown --style technical-schematic

# 2. 写规范 / 流程 / SOP（竖版利于手机阅读）
--layout linear-progression --style ikea-manual --aspect portrait

# 3. 项目状态看板 / KPI 汇报
--layout dashboard --style corporate-memphis
```

其它常用参数：
- `--lang zh|en|ja`
- `--aspect landscape|portrait|square` 或自定义如 `3:4`、`2.35:1`
- `--ref <files>` 提供参考图（direct 直传 / style 抽风格 / palette 抽配色）
- `--no-confirm` 跳过确认门（仅在批量出图时使用）

---

## 7. 避坑提示

- **避免使用**：`kawaii`、`claymation`、`storybook-watercolor`、`pixel-art`、`lego-brick`、`origami` —— 风格过于活泼，向上汇报会拉低专业度
- **慎用**：`cyberpunk-neon` 仅用于对外发布会 / 技术品牌物料，日常汇报显浮夸
- **画幅选择**：
  - 横版 `landscape (16:9)`：日常汇报、PPT 嵌入、飞书文档
  - 竖版 `portrait (9:16)`：制度类、深度普及，适合手机端阅读和群内传播
- **凭据安全**：skill 已内置剥离 API Key / Token 的规则，但贴源文件前仍需自查，避免把 `.env` 或带密钥的截图喂进去
- **确认门**：默认每次生成前都会让你确认布局 / 风格 / 比例 / 语言 / 后端，不要图快加 `--no-confirm`，除非你已批量验证过组合

---

## 8. 输出目录结构（每次生成都会产出）

```
infographic/{topic-slug}/
├── source-{slug}.{ext}        # 原文备份
├── analysis.md                # 内容分析
├── structured-content.md      # 结构化内容
├── prompts/infographic.md     # 最终 prompt（可复现 / 可换后端重跑）
└── infographic.png            # 成图
```

文件冲突时自动加 `-YYYYMMDD-HHMMSS` 时间戳备份，不会覆盖。

---

## 9. 触发方式速记

- 自然语言："帮我做一张关于 X 的信息图 / 高密度信息大图 / 可视化"
- 关键词快捷方式：
  - `信息图 / infographic` → 默认 `bento-grid + craft-handmade`，横版
  - `高密度信息大图` → 默认 `dense-modules + morandi-journal/pop-laboratory/retro-pop-grid`，竖版
- 显式调用：直接说"使用 baoyu-infographic skill"或在 Codex 中调起对应 skill

---

## 附：场景 → 命令速查表

| 我要做什么 | 一键命令片段 |
|---|---|
| 周报 / 月报 | `--layout dashboard --style corporate-memphis` |
| 季度路线图 | `--layout winding-roadmap --style corporate-memphis` |
| 研发规约 / SOP | `--layout linear-progression --style ikea-manual --aspect portrait` |
| 管理层级图 | `--layout hierarchical-layers --style corporate-memphis` |
| 系统架构图 | `--layout structural-breakdown --style technical-schematic` |
| 调用链 / 数据流 | `--layout subway-map --style technical-schematic` |
| 重构前后对比 | `--layout binary-comparison --style ui-wireframe` |
| 微服务拓扑 | `--layout isometric-map --style pop-laboratory` |
| 概念科普 | `--layout hub-spoke --style hand-drawn-edu` |
| 深度技术长图 | `--layout dense-modules --style pop-laboratory --aspect portrait` |
| 内部技术分享预热 | `--layout bento-grid --style chalkboard` |
| 对外传播长图 | `--layout dense-modules --style retro-pop-grid --aspect portrait` |
