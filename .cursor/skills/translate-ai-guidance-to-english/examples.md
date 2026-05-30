# Examples

## Example 1: Prompt

### Japanese Source

```text
新卒にも分かるように、なるべく丁寧に説明してください。
最初に結論を書いてください。
```

### English Version

```text
MUST start with the conclusion.
SHOULD explain the topic in a way that a new graduate can understand.
When using technical terms, include a short plain-language explanation and one concrete example.
Do not increase the explanation length unless it helps the reader decide the next action.
```

## Example 2: Cursor Rule

### Japanese Source

```text
非同期処理にはUniTaskを使う。
Task.Delayは使わない。
```

### English Version

```markdown
---
description: Unity async rules for C# scripts
globs: "**/*.cs"
alwaysApply: false
---

# Unity Async Rules

- MUST use UniTask for asynchronous operations.
- NEVER use `Task.Delay`.
- SHOULD use `UniTask.Delay` when a delay is required in Unity runtime code.
```

## Example 3: Agent Skill

### Japanese Source

```text
レビューしてと言われたら、問題点を先に出す。
問題がない場合は問題なしと言う。
```

### English Version

```markdown
---
name: review-findings-first
description: Reviews changes and reports issues before summaries. Use when the user asks for a review, code review, specification review, or quality check.
---

# Review Findings First

## Instructions

When reviewing:

1. Start with `Issues found`, `No issues found`, or `Needs confirmation`.
2. If issues exist, list them before any summary.
3. Order issues by severity.
4. Include the affected file, behavior, and recommended fix.
5. If no issues are found, state that clearly and mention remaining test gaps.
```

## Example 4: Human Confirmation

### Japanese Source

```text
いい感じに読みやすくしてください。
```

### English Version

```text
SHOULD improve readability by:
1. Using one instruction per sentence.
2. Grouping related instructions under short headings.
3. Replacing vague terms with concrete conditions.

Do not change the original intent or add new requirements.
```

### Human Confirmation Needed

- Define whether "readable" means shorter, more beginner-friendly, more strict, or easier for AI agents to execute.

## Example 5: MDC Optimization

### Japanese Source

```text
AIはレビュー依頼では問題点を先に出す。
MarkdownではH1を1回だけ使う。
Unity C#では[SerializeField] privateを使い、public fieldは避ける。
```

### Recommended Rule Files

| File | Activation | Purpose |
| --- | --- | --- |
| `.cursor/rules/review-findings-first.mdc` | agent-requested | Applies when the user asks for a review |
| `.cursor/rules/markdown-format.mdc` | globs: `"**/*.md"` | Applies to Markdown documents |
| `.cursor/rules/unity-csharp-fields.mdc` | globs: `"**/*.cs"` | Applies to Unity C# scripts |

### MDC Files

#### `.cursor/rules/review-findings-first.mdc`

```markdown
---
description: Use when reviewing code, specifications, plans, or documentation
alwaysApply: false
---

# Review Findings First

- MUST start review responses with `問題あり`, `問題なし`, or `要確認`.
- MUST list findings before summaries when issues exist.
- SHOULD order findings by severity.
```

#### `.cursor/rules/markdown-format.mdc`

```markdown
---
description: Markdown heading rules for project documentation
globs: "**/*.md"
alwaysApply: false
---

# Markdown Heading Rules

- MUST use exactly one H1 heading per Markdown document.
- SHOULD use H2 and lower headings in order without skipping levels.
```

#### `.cursor/rules/unity-csharp-fields.mdc`

```markdown
---
description: Unity C# field exposure rules for Inspector references
globs: "**/*.cs"
alwaysApply: false
---

# Unity C# Field Rules

- MUST use `[SerializeField] private` for Inspector references.
- NEVER use public fields only to expose values in the Inspector.
```

### Human Confirmation Needed

- Confirm whether the Unity rule should apply to all C# files or only `Assets/**/*.cs`.
