---
name: pr-precheck
description: Pre-submission due diligence for upstream PRs to public/large repositories. Use this before opening a PR — especially in repos with deep PR backlogs (hundreds+ open) — to avoid duplicating existing work, sidestepping the actual root cause, or building on stale or deprecated architecture. Triggers on phrases like "提个 PR", "want to contribute", "open a PR upstream", "fix this in <repo>", "PR 前的尽职调查". See references/checklist.md for the full procedure and references/gh-recipes.md for ready-to-paste search commands.
---

# PR Precheck

提交上游 PR 前的尽职调查 skill。**目标**：让你的 PR 不重复、不走偏、不过时、不被静默忽略。

## 🛑 硬规则：对外大动作必须先停下等用户确认

**在执行下列任何动作之前，必须先暂停并向用户复述将要做什么、得到明确"动手"信号才执行：**

- 创建 PR（`gh pr create`）
- 关闭 PR（`gh pr close`）
- 在他人 PR / Issue 下评论（`gh pr comment`、`gh issue comment`）
- 推送到非自有仓库分支（`git push` 到 fork 也算）
- 任何会出现在他人通知/邮箱里的操作

**理由**：这些动作会消耗仓库 maintainer 的注意力预算、留下永久痕迹、有时不可撤回。即使调研结论看起来很确定，最后的"按下发送"动作权属于人类——agent 应当呈现完整草稿（标题、body、目标 PR 号、关闭理由等）等待审核。

**反模式**：autonomous mode 下"反正用户授权了就一路做完"——不行。autonomous 是说不需要逐步确认探索性操作，不是说可以自动 broadcast。

**例外**：仅对自有仓的本地操作（local commit、push 到自己 fork 的私有分支）不需要逐项确认；但只要消息会进入第三方视野，回到主规则。

## 何时触发

- 计划往一个非自有仓库提 PR（尤其大仓 / 高 PR backlog 的项目）
- 看到一个 bug 想"顺手修了提个 PR"
- 之前的 PR 被 reviewer 标为 duplicate、out-of-architecture、obsolete

## 核心检查清单（跳过任何一步都可能浪费数小时）

### 1. 验证问题真实存在于**最新 main**

不是你本地分支、不是你 fork、不是几周前的 main。

```bash
git fetch origin main && git log origin/main --oneline | head -5
```

读相关代码在 `origin/main` 上的当前状态。问题可能已被修复、绕过、或被新架构替代。

### 2. 排查上游"自动机制"是否已覆盖

许多大型项目有运行时自动发现机制，可能让你的"修复"变成 no-op：

- **远程模型/能力列表**（models.dev、provider 自带 `/models` 端点、OpenRouter 实时元数据）
- **Profile/Plugin 注册表**（profile 默认行为可能已经处理你认为没处理的 case）
- **配置文件自动检测**（项目类型嗅探、能力 flag 推导）
- **环境变量/Feature flag**（功能可能被门控但已实现）

明确读相关入口函数（`_supports_*`、`fetch_*_metadata`、`build_*_extras` 等），确认数据流真的会落到你想修的代码路径。

### 3. PR/Issue 全景调研（**最容易翻车的一步**）

不要只搜标题关键词。你的目标是找到所有人对**同一个底层问题**的尝试，无论他们怎么命名。

**多维度搜索**（见 references/gh-recipes.md）：

- 按文件路径搜：`gh pr list --search "<file/path/of/the/fix>"`
- 按代码符号搜（如类名、函数名）
- 关键词搜 + 同义词：`thinking` / `reasoning` / `chain-of-thought`、`plumb` / `pass` / `support` / `add`
- **同时搜 `--state open` 和 `--state closed`**——关闭未合的 PR 告诉你 maintainer 拒绝了什么、或者这块"内部正在重写"
- **搜 issues**——bug 可能从用户痛点角度被描述
- 看每个候选的 `--json files` 列表，相同文件的 PR 99% 是重叠

### 4. 逐个 diff 候选 PR

不靠标题脑补。`gh pr diff <num>` 直接对比代码。问自己：

- 我打算改的代码块，是否已经被某个 PR 改成几乎一样？
- 我的方案有没有任何**真实可证的**差异化（不是修辞，而是 diff 行为）？
- 别人的方案有没有处理我漏掉的边界？

### 5. 检查相邻/依赖 PR 链

修复可能由多个 PR 组成。问：

- 你的 fix 依赖另一个 PR 先合（否则是死代码）？
- 别人的 PR 看似只覆盖一半，是因为另一半在另一个 PR 里？
- 是否存在一个"全包"PR 已被关闭——多半暗示 maintainer 偏好拆小步走

### 6. 架构对齐度

- 项目的核心架构最近是否变化（重构、迁移、弃用旧 API）？
- 你的修复用的是 **新模式** 还是 **老模式**？老模式的 PR 即便正确也会被推翻。
- 看最近合并的 PR 关键词、看 CHANGELOG 里的 "Breaking" 段、看有没有 "deprecated" 注释。

### 7. 闭环价值复核

- 你的 PR 真正解决了用户痛苦吗？还是只动了表层？
- 如果有更深的 bug（比如 multi-turn replay、auxiliary path），你的 PR 不覆盖它，maintainer 会优先等"全方位方案"。
- 这种情况要么扩大 scope，要么在 PR 描述里**明确声明** scope 边界并指向相关 issue/PR。

## 走偏的常见信号

| 信号 | 含义 |
|---|---|
| 13+ 同 scope PR 都 OPEN 没人审 | 这块 maintainer 不打算外部合，可能内部要重写——别投入 |
| 最全面那一版被关 | 上游偏好拆小步，或拒绝大改动；找最小可合的切片 |
| 很多 PR 用 `is_xxx` 布尔 flag，但 main 已转向 profile/plugin | 老模式 PR 全部过时；你写新模式有差异化 |
| 仓库根本没收录你修复的模型/case | 上游可能依赖第三方目录（models.dev 等），先去那里加 |

## 决策矩阵

调研完后落到三选一：

1. **不提 PR**——已有等价 PR、或问题不真实存在、或这块 maintainer 故意冷处理
2. **直接在已有 PR 评论**——补一个真实可证的边界、或贴一段验证数据，不刷存在感
3. **提自己的 PR**——必须能用一句话说清"这与现有 X 个 PR 的差异是 Y"

## 参考

- `references/checklist.md` — 可打印的逐步表单
- `references/gh-recipes.md` — 现成的 `gh` 命令模板（按文件搜、按状态搜、批量 diff 等）
- `references/anti-patterns.md` — 真实案例：哪些"想当然"的修复其实是 no-op
