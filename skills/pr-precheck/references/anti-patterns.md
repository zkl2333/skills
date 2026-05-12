# 反模式与真实案例

记录从踩过的坑里抽出的"想当然"模式，下次能在 24 小时之前停下。

## 反模式 1：只搜标题就以为查全了

**场景**：在 hermes-agent 提 DeepSeek thinking 模式 PR，搜 `"deepseek thinking in:title"` 找到 13 个 OPEN，自以为已经覆盖。

**实际**：另一个 PR 标题是 `"native thinking/reasoning_effort support"`——不含 "deepseek" 也不含 "thinking" 一起出现的形式。它的 diff 与你的几乎一字不差。

**教训**：**永远再按文件路径搜一遍**。`gh pr list --search "<file/path>"`。同一文件被改的 PR 99% 是重叠。

---

## 反模式 2：看到很多人没合就觉得"反正都没合，我提一下也无妨"

**场景**：13+ 同 scope PR 全部 OPEN 0 review。

**实际**：这种"集体静默"是强信号——maintainer 不打算从外部合并，可能内部正在重写、可能在等其他 PR 落地、可能这块被冷处理。再加一个 PR 不会改变这个状态，只会增加你的 PR-graph 调研负担。

**教训**：**集体静默 = 别投入**。换个冷门、单点、明确无主的 bug 才有正反馈。

---

## 反模式 3："我有架构优势"自我安慰

**场景**：发现别人都用老的 `is_xxx` flag 模式，你用新的 profile 架构，于是觉得"我的差异化够强"。

**实际**：对的，是有差异化。但 maintainer 没在审这块——架构正确性救不了"被忽略"的命运。而且第二个走 profile 路线的人会用同样的话术，你的差异化 24 小时后就消失。

**教训**：差异化要**对用户可观测**（修了某个其他 PR 没修的边界、覆盖了某个被遗漏的 case），架构纯洁性不算。

---

## 反模式 4：以为"上游模型列表机制"会自动覆盖

**场景**：用户问"会不会某个拉远程模型列表的机制已经处理了？"——你直觉说"可能"，没去验证。

**实际**：去读 `_supports_reasoning_extra_body()`，发现它对 native DeepSeek 直接 return False（gate 在 `if "openrouter" not in self._base_url_lower: return False`）。再去看 models.dev，发现里面**根本没有 deepseek 的 v4 条目**。所谓"自动机制"在这个场景下完全不起作用。

**教训**：**任何"它可能已经处理了"都要去 grep 验证**。运行时数据流读一遍，别靠猜。

---

## 反模式 5：修了表层、漏了真痛点

**场景**：你的 PR 解决了"`reasoning_effort` 被静默丢弃"。但同时存在更深的 bug：multi-turn 时 `reasoning_content must be passed back` 的 400 错误（auxiliary path、cron path、tool-call replay 等多个位置）。这个深 bug 让用户**真的用不起来**。

**实际**：maintainer 看 PR 队列时，会优先等覆盖深 bug 的"全方位方案"，你的浅层 fix 会被一直推。

**教训**：先找清楚"真正阻塞用户的痛点是什么"，不要修表层让自己有成就感。如果只能修一部分，PR 描述里**明确指出 scope 边界**并指向更深的 issue/PR——这至少让 maintainer 知道你看到了全景。

---

## 反模式 6：没看 closed PR

**场景**：只看 OPEN，错过了"最全面那一版"已经被关闭的事实。

**实际**：那个被关的 PR 暗示了 maintainer 的偏好（"不要大包大揽"或"这块要内部重写"或"已经被另一个机制接管"），是极其重要的负面信号。

**教训**：`--state all`，永远。closed-without-merge 的 PR 比 OPEN 的更有信息量。

---

## 反模式 7：发现 duplicate 后还想"补一个有价值的评论"

**场景**：reviewer 标 duplicate，你想去 earliest PR 留言指出某个边界 bug，刷一次存在感+体现价值。

**实际**：除非那个边界 bug 真实可验且对当前 PR 的合并起决定作用，否则就是噪声。Maintainer 时间稀缺，每个评论都在消耗 attention budget。

**教训**：**少即是多**。不刷存在感、不解释、不"补充"。该关就关，干净退场。下次贡献机会找差异化更强的地方。
