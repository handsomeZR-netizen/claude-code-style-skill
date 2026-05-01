# claude-code-style

> Make Codex sound like Claude Code: concise, truthful, tool-disciplined.

A single-file skill that ports the narrative discipline of Anthropic's Claude Code to Codex (or any tool that reads an `AGENTS.md`).

## What this is

Out of the box, most coding assistants are verbose, agree-y, and over-explainy. They open with "Great question!", narrate their internal deliberation, hedge with "this might possibly be...", drop time estimates, and leave `PLAN.md` debris behind. Claude Code doesn't do those things. This skill is a single Markdown file that captures *why* and writes the rules down so Codex can follow them too.

It's about thirteen rules plus a decision tree, distilled from observed Claude Code behavior. The rules are all about output discipline — length matched to question size, no theater, truth over agreement, tool discipline. There is no code, no install scripts, no dependencies. Just prose.

## 30-second install

**Per-session.** Drop `SKILL.md` into your skills folder and trigger it with `$claude-code-style`:

```text
$claude-code-style help me debug this auth handler
```

**Always-on.** Copy everything from the H1 (`# Claude Code interaction style`) down — the body of `SKILL.md`, **not** the YAML frontmatter — into `~/.codex/AGENTS.md`:

```bash
# crude version: skip the YAML, append the rest
sed -n '/^# Claude Code interaction style/,$p' SKILL.md >> ~/.codex/AGENTS.md
```

The frontmatter `name` / `description` fields are skill-discovery metadata and don't belong in `AGENTS.md`.

## Before / after teaser

Same prompt — *"What does `git restore --staged` do?"* — answered two ways:

> **Default chatty assistant.** Great question! `git restore --staged` is a really useful command that you'll find handy in your daily workflow. It's used to unstage files that you've previously added to the staging area. Essentially, it's the modern replacement for `git reset HEAD <file>`. Let me know if you'd like me to explain how it differs from `git restore` without `--staged`!

> **Claude-code-style.** `git restore --staged <path>` unstages a file — moves it from the index back to the working tree, content unchanged. Modern replacement for `git reset HEAD <path>`.

Three more contrasts (code-change request, exploratory question, risky operation) live in [`SKILL.md` — Before / After examples](./SKILL.md#before--after-examples).

## What it changes about your assistant

- No emojis, no "you're absolutely right", no "this should take a few minutes"
- One-sentence preamble before the first tool call, no internal-monologue narration
- `path/file.ext:line` references instead of "somewhere near the top of process.ts"
- 1–2 sentence end-of-turn summary, not a 6-paragraph postscript
- Numbered steps or short tables when the answer is genuinely multi-dimensional, plain prose otherwise
- No `PLAN.md` / `NOTES.md` / `SUMMARY.md` left behind unless you asked for one
- Independent tool calls fired in parallel, dependent calls run sequentially
- Confirmation before push, force-update, deploy, or anything that touches shared state
- Disagrees when the evidence disagrees — does not validate beliefs the data contradicts

## Codex specifics

Claude Code has `TodoWrite` / `Read` / `Edit` / `Write` / `Glob` / `Grep`; Codex has `update_plan` / `apply_patch` / `shell` (`rg`, file reads). The principles transfer; the specific tool names don't. See [`SKILL.md` — Codex adaptation notes](./SKILL.md#codex-adaptation-notes) for where the analogy bends and where it doesn't.

## Attribution

Distilled from observed Claude Code behavior. Not affiliated with or endorsed by Anthropic. "Claude Code" is Anthropic's product; this skill is an independent attempt to teach the same discipline to other tools.

## License

MIT — see [`LICENSE`](./LICENSE).

## Contributing

PRs welcome for tightening rules, fixing edge cases, or adding crisp before/after examples. Please don't expand the scope beyond Claude-Code-style narrative discipline — the skill stays useful only as long as it stays small.

---

## 中文版

> 让 Codex 说话像 Claude Code：短、诚实、工具有纪律。

一份单文件 skill，把 Claude Code 的叙事纪律移植给 Codex（或任何读 `AGENTS.md` 的工具）。

### 这是什么

主流 LLM 编码助手默认输出冗长、谄媚、过度解释。开口就是 "Great question!"，写一大段内心独白，"我觉得这可能或许是……" 半天不下结论，再随口给个 "几分钟就好" 的时间估计，留下一堆 `PLAN.md` 残骸。Claude Code 不这样。这份 skill 用一个 Markdown 文件把 *为什么* 和具体规则一起写下来，让 Codex 也能照着做。

约 13 条规则 + 决策树 + 自检清单，从对 Claude Code 的实际观察里萃取。规则全部是输出纪律 —— 篇幅匹配问题大小、不演戏、宁实勿和、工具调用有纪律。没有代码、没有安装脚本、没有依赖，只有 prose。

### 30 秒安装

**单次触发**：把 `SKILL.md` 丢进 skills 目录，用 `$claude-code-style` 触发：

```text
$claude-code-style 帮我看看这个 auth handler 有什么问题
```

**长期生效**：把 `SKILL.md` 里 H1（`# Claude Code interaction style`）以下的全部内容——也就是 body，**不要带 YAML frontmatter**——粘到 `~/.codex/AGENTS.md`：

```bash
sed -n '/^# Claude Code interaction style/,$p' SKILL.md >> ~/.codex/AGENTS.md
```

frontmatter 的 `name` / `description` 是 skill 检索用的元数据，不属于 `AGENTS.md`。

### 一组对照

同一个问题——*"`git restore --staged` 是干嘛的？"*——两种回答：

> **默认啰嗦版**：好问题！`git restore --staged` 是日常工作流里非常有用的命令。它的作用是把你之前 `git add` 进暂存区的文件取消暂存。本质上是 `git reset HEAD <file>` 的现代替代写法。如果你想了解它和不带 `--staged` 的 `git restore` 的区别，请告诉我！

> **Claude-code 版**：`git restore --staged <path>` 取消暂存——把文件从 index 移回工作树，内容不变。是 `git reset HEAD <path>` 的现代写法。

另外 3 组对照（代码改动 / 探索性问题 / 危险操作）见 [`SKILL.md` 的 Before / After examples](./SKILL.md#before--after-examples)。

### 致谢与许可证

基于对 Claude Code 行为的观察总结，与 Anthropic 无官方关联。"Claude Code" 是 Anthropic 的产品，本仓库是把同一套纪律教给其他工具的独立尝试。

许可证：MIT，见 [`LICENSE`](./LICENSE)。

欢迎提 PR 收紧规则或加 before/after 示例。请不要扩 scope —— 这份 skill 只有保持小才有用。
