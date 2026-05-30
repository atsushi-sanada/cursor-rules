# MDC Optimization

Use this reference when converting Japanese guidance into Cursor `.mdc` rule documents.

## Goal

Create rules that Cursor can attach at the right time without wasting context.

Good `.mdc` rules are:

- Focused on one concern.
- Scoped to the right files or situations.
- Written as concrete instructions.
- Short enough to stay useful in context.
- Easy to discover from filename and description.

## Format Boundary

Cursor User Rules and Cursor Project Rules are different formats.

- User Rules are plain text in Cursor Settings. If a list shows only the first line, put a readable rule identifier on the first line.
- Project Rules live in `.cursor/rules/*.mdc`. They require `.mdc` frontmatter metadata at the top of the file. Do not put a filename line before the opening `---`.
- AGENTS.md is plain Markdown and can use a first-line title.

If the user asks for both first-line visibility and `.mdc` behavior, explain that these requirements conflict. Provide a plain User Rule variant for first-line visibility and a `.mdc` Project Rule variant for scoped Cursor rule behavior.

## Decision Flow

### 1. Decide Whether It Should Be a Rule

Create a `.mdc` rule when the guidance should persist across chats.

Do not create a `.mdc` rule when:

- The instruction is one-time task context.
- The content is a long specification.
- The content is a project document that should stay in README or docs.
- The guidance needs a multi-step workflow; use a Skill instead.

### 2. Split the Source

Split the Japanese source into multiple `.mdc` files when it contains more than one concern.

| Source Concern | Recommended Rule |
| --- | --- |
| Response style | `ai-response-style.mdc` |
| Review reporting | `review-findings-first.mdc` |
| Markdown format | `markdown-format.mdc` |
| File naming | `file-naming.mdc` |
| Unity C# coding | `unity-csharp-coding.mdc` |
| Python coding | `python-coding.mdc` |

Keep together only rules that are always needed together.

### 3. Choose Activation Mode

#### Always Apply

Use only for project-wide rules that must affect every chat.

```yaml
---
description: Core AI response behavior for this project
alwaysApply: true
---
```

Use sparingly. Too many always-on rules waste context and can conflict.

#### Apply to Specific Files

Use for language, framework, or directory-specific rules.

```yaml
---
description: Unity C# coding rules for scripts
globs: "**/*.cs"
alwaysApply: false
---
```

Use this mode by default for coding standards.

#### Agent Requested

Use when the rule applies to a recognizable situation but not a specific file pattern.

```yaml
---
description: Use when reviewing code, specifications, or implementation plans
alwaysApply: false
---
```

The description must be specific enough for the agent to decide when to load it.

#### Manual

Use when the rule should load only when the user explicitly mentions it.

```yaml
---
alwaysApply: false
---
```

Do not use manual mode for rules that are required for safety or correctness.

### 4. Name the File

Use lowercase words separated by hyphens.

Good:

- `unity-csharp-coding.mdc`
- `review-findings-first.mdc`
- `markdown-format.mdc`

Avoid:

- `rule.mdc`
- `common.mdc`
- `いい感じルール.mdc`
- `UnityCSharpCodingRules.mdc`

### 5. Write the Description

The description must answer:

- What does this rule cover?
- When should Cursor apply it?

Good:

```yaml
description: Unity C# coding rules for MonoBehaviour scripts, serialization, lifecycle methods, and runtime performance
```

Weak:

```yaml
description: Unity rules
```

### 6. Write the Body

Use direct instructions:

- `MUST` for required behavior.
- `SHOULD` for default behavior with valid exceptions.
- `MAY` for optional behavior.
- `NEVER` for prohibited behavior.

Keep examples short.

For `.mdc` files, use the first Markdown heading after frontmatter to identify the rule:

```markdown
# unity-csharp-fields.mdc - Unity C# Field Rules
```

For User Rules or other plain Markdown rules, put the identifier on the first line:

```markdown
# unity-csharp-fields - Unity C# Field Rules
```

Prefer:

```markdown
- MUST use `[SerializeField] private` for Inspector references.
- NEVER use public fields only to expose values in the Inspector.
```

Avoid:

```markdown
Please write good code that is easy to understand.
```

### 7. Output Contract

When generating `.mdc` files, output this structure:

```markdown
## Recommended Rule Files

| File | Activation | Purpose |
| --- | --- | --- |
| `.cursor/rules/example-rule.mdc` | globs: `"**/*.ts"` | TypeScript coding rules |

## MDC Files

### `.cursor/rules/example-rule.mdc`

\```markdown
---
description: TypeScript coding rules for application source files
globs: "**/*.ts"
alwaysApply: false
---

# example-rule.mdc - TypeScript Coding Rules

- MUST ...
- SHOULD ...
- NEVER ...
\```

## Human Confirmation Needed

- Confirm whether the rule should apply to `**/*.tsx` as well.
```

### 8. Final Checklist

- [ ] Each `.mdc` file has one concern.
- [ ] Filename is clear and lowercase hyphen-separated.
- [ ] Frontmatter is valid YAML.
- [ ] Activation mode is appropriate.
- [ ] `description` is specific.
- [ ] `globs` are quoted when present.
- [ ] `alwaysApply: true` is used only for universal rules.
- [ ] Body is actionable and concise.
- [ ] Large source rules are split instead of copied as one large rule.
- [ ] Human confirmation items are listed for unclear scope or globs.
