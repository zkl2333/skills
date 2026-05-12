# PR Precheck 检查清单

逐项打勾。任何一项不过都先停下，不要急着写代码。

## A. 问题真实性

- [ ] 已 `git fetch origin main && git checkout origin/main`（或等价同步）
- [ ] 在最新 main 上**复现了 bug**（或读了相关代码，确认 bug 真实存在）
- [ ] 排除了"自动机制已覆盖"——读了相关入口函数，确认数据流真的会落到我打算修的代码

## B. PR/Issue 全景

- [ ] 按**文件路径**搜了所有相关 PR（`gh pr list --search "<path>"`）
- [ ] 按**关键代码符号**（类名、函数名、常量）搜了所有相关 PR
- [ ] 用**多个同义词**搜了关键词（thinking/reasoning/CoT、plumb/pass/support）
- [ ] 同时搜了 `--state open` 和 `--state closed`
- [ ] 搜了 issue tracker（不只是 PR）
- [ ] 找到的每个候选 PR：用 `gh pr diff <num>` **看了实际 diff**

## C. 候选差异化

- [ ] 我能用**一句话**说清我的 PR 与每个现有候选的具体差异
- [ ] 这个差异是**diff 行为差异**，不是描述/文档差异
- [ ] 这个差异**对用户可观测**，不是纯重构偏好

## D. 依赖与架构

- [ ] 我的 fix 不依赖另一个未合 PR 才能生效（否则是死代码）
- [ ] 我用的代码模式与 main 上**最近 3 个月**合并的 PR 风格一致
- [ ] 没有 `# DEPRECATED` / `# legacy` 注释暗示我用的入口要被移除
- [ ] 检查 CHANGELOG / 最近 commits 里有无 "Breaking" / "Refactor" 提示架构迁移

## E. Scope 与价值

- [ ] 我的 PR **真正终结了某个用户可见的痛点**——不是修了一半
- [ ] 如果只修了一部分，PR 描述里**明确写出 scope 边界**并指向相关 issue
- [ ] 不掺其他无关清理（"顺手"修的东西另开 PR）

## F. 信号检查

如果命中下列任意一条，**严重警告**——大概率提了也白提：

- [ ] 同 scope 已有 5+ open PR 全部 0 review
- [ ] 最近 3 个月这个目录有大型重构 commit
- [ ] 已存在的"最全面"版本被关闭
- [ ] Maintainer 在某个 issue 里说过"will be addressed in upcoming refactor"

## G. 提交前的最后一步

- [ ] 已基于 latest origin/main 重新 rebase
- [ ] 跑了相关测试套件
- [ ] PR 描述包含：bug 是什么、为什么这是正确解、与现有 PR 的差异（如果有）、test plan
- [ ] 准备好了被关闭/duplicate 标记的预案（不情绪化、不刷存在感）
