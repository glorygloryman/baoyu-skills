# 定制 baoyu-infographic 并部署到 Codex 操作手册

> 适用前提：你已经 fork 了 `JimLiu/baoyu-skills` 到自己的 GitHub 账号（例如 `glorygloryman/baoyu-skills`），并 clone 到本地。
> 创建日期：2026-04-30
> 用途：作为 fork 仓库后自定义 skill 内容（增删布局/风格、加品牌色、改默认值）并部署到 Codex 的标准操作流程。

---

## 三种定制方案对比

| 维度 | A. 仅 EXTEND.md | B. 用户级 Fork | C. 私有仓库 Fork（本手册） |
|---|---|---|---|
| 改动量 | 5 行配置 | 改 SKILL.md + 删几个 md | 完整 fork + 发布 |
| 真正"删掉"选项 | ❌（只是不推荐） | ✅ | ✅ |
| 多机同步 | 手动复制 EXTEND.md | 手动复制 `~/.baoyu-skills/` | git pull / 重装插件 |
| 团队共享 | ❌ | ❌ | ✅ |
| 跟随上游更新 | ✅ 自动 | ⚠ 手动 merge | ⚠ git rebase upstream |
| 自定义新风格 | 用 `custom_styles` 字段 | ✅ 直接加 md | ✅ 直接加 md |

**已 fork → 直接走方案 C 最干净**。

---

## 标准操作流程（8 步）

### Step 1：设置 upstream 以便同步原作者更新

```bash
git remote add upstream https://github.com/JimLiu/baoyu-skills.git
git remote -v
# 应看到 origin（你的 fork）和 upstream（原作者）
```

### Step 2：开定制分支（不要在 main 上直接改）

```bash
git checkout -b customize/infographic-trim
```

理由：main 保持镜像状态便于追上游；定制工作隔离在分支中，rebase 更容易。

### Step 3：裁剪 `skills/baoyu-infographic/`

```bash
cd skills/baoyu-infographic/references

# 删除不需要的布局定义文件
rm layouts/{tree-branching,iceberg,funnel,periodic-table,comic-strip,story-mountain,jigsaw,venn-diagram,hierarchical-layers}.md

# 删除不需要的风格定义文件
rm styles/{claymation,kawaii,storybook-watercolor,origami,pixel-art,lego-brick,craft-handmade,cyberpunk-neon,bold-graphic,morandi-journal,retro-pop-grid,hand-drawn-edu}.md
```

### Step 4：编辑 `skills/baoyu-infographic/SKILL.md`

修改以下四处：

1. **顶部 description**：把 `21 layout types and 21 visual styles` 改成实际保留数量（例如 `12 layout types and 9 visual styles`）
2. **Layout Gallery 表格**：删除已删 md 对应的行
3. **Style Gallery 表格**：删除已删 md 对应的行
4. **默认值**：
   - `--layout` 默认 `bento-grid`（保留）
   - `--style` 默认从 `craft-handmade` 改成 `corporate-memphis`
5. **Recommended Combinations 表**：删除引用了已删项的行
6. **Keyword Shortcuts 表**：把映射到已删项的风格替换成保留的风格

### Step 5：本地验证

```bash
ls skills/baoyu-infographic/references/layouts/ | wc -l
ls skills/baoyu-infographic/references/styles/  | wc -l
```

数量对得上即生效。在项目目录起 Claude/Codex 会话调用 skill 时会自动加载（项目级 `skills/` 优先级最高）。

### Step 6：提交并推送到你的 fork

```bash
git add skills/baoyu-infographic/
git commit -m "customize(infographic): trim layouts/styles for tech-management use"
git push -u origin customize/infographic-trim
```

建议 commit 粒度细一点（删布局一个 commit、删风格一个 commit、改 SKILL.md 一个 commit），便于将来 rebase 时判断该保留哪些 hunk。

### Step 7：在 Codex 中安装

Codex CLI 安装 plugin 的方式（具体命令以 `codex plugin --help` 为准）：

```bash
# 方式 A：从 GitHub fork 安装（推荐，多机同步）
codex plugin install github:glorygloryman/baoyu-skills@customize/infographic-trim

# 方式 B：本地路径安装（本机开发期，改完即生效）
codex plugin install /Users/cy/MyWorkFactory/workspace/OpenSource/baoyu-skills

# 方式 C：marketplace.json 形式
codex marketplace add /Users/cy/MyWorkFactory/workspace/OpenSource/baoyu-skills/.claude-plugin/marketplace.json
```

不同 Codex 版本子命令可能略有差异（`install` / `add` / `enable`），但"指向你的 fork 路径或仓库"这个核心不变。

### Step 8：日常维护——同步上游

每过一段时间（看 upstream 发版）：

```bash
git fetch upstream
git checkout main
git merge upstream/main
git push origin main

git checkout customize/infographic-trim
git rebase main
# 处理 SKILL.md 冲突（上游改了 Gallery 你也改过 → 大概率冲突）
git push --force-with-lease origin customize/infographic-trim
```

`--force-with-lease` 比 `--force` 安全：远端被别人推过会拒绝，避免覆盖。

---

## 高阶：自定义品牌风格

```bash
# 在 references/styles/ 加一个新文件
touch skills/baoyu-infographic/references/styles/my-company.md
```

按现有 style 文件格式写入：品牌色（hex）、字体、视觉规则、构图偏好。

然后在 `SKILL.md` 的 Style Gallery 表加一行：

```markdown
| `my-company` | 公司品牌视觉，主色 #XXXXXX |
```

重启 Codex / Claude 会话即可使用 `--style my-company`。

> 也可以不改 skill 文件、只在 `EXTEND.md` 里用 `custom_styles` 字段定义——更轻量但只对自己生效，团队共享需走 fork 路径。

---

## 几条踩坑提示

1. **不要直接删除 `Deprecated Skills` 段落**（CLAUDE.md 里的 `baoyu-image-gen`、`baoyu-xhs-images` 说明）——是原作者的内部说明，留着方便同步上游
2. **marketplace.json 的版本号别乱改**——除非你打算独立发布；保留原版本便于追溯
3. **如果只用 `baoyu-infographic`**，可以从 marketplace.json 的 `plugins.skills` 数组删掉其它 skill 路径，但**不要删 `skills/` 目录下的文件**，万一以后想用方便恢复
4. **commit 信息用 `customize(...)` 前缀**——和原作者的 `feat/fix/chore` 区分开，rebase 时一眼看出哪些是你自己的改动
5. **Force push 只用 `--force-with-lease`**，不用裸 `--force`

---

## 团队部署模式

如果要让团队成员都用你定制的版本：

1. 把 `customize/infographic-trim` 分支合并到自己 fork 的 main（自己审自己 PR）
2. 团队成员安装：
   ```bash
   codex plugin install github:glorygloryman/baoyu-skills
   # 默认拉 main 分支，自动获得你的定制
   ```
3. 上游有更新时，你做完 Step 8 的同步流程后推到 main，团队成员 `codex plugin update baoyu-skills` 即可

---

## 速查命令

```bash
# 查看当前定制状态（哪些 layout/style 文件还在）
ls skills/baoyu-infographic/references/layouts/ skills/baoyu-infographic/references/styles/

# 与上游 SKILL.md 对比差异
git fetch upstream
git diff upstream/main -- skills/baoyu-infographic/SKILL.md

# 回滚到上游版本（放弃定制）
git checkout upstream/main -- skills/baoyu-infographic/
```
