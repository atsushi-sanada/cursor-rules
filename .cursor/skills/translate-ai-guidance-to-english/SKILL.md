---
name: translate-ai-guidance-to-english
description: Converts Japanese prompts, Cursor Rules, and AI Agent Skills into clear English instructions for AI coding agents. Also designs optimized Cursor .mdc rule documents, including rule splitting, activation mode selection, frontmatter, globs, descriptions, and filenames. Use when the user asks to English-translate, rewrite, optimize, mdc-ify, rule-ify, or convert Japanese AI guidance, prompts, rules, .mdc files, or SKILL.md files for Cursor, Claude, GPT, or coding agents.
disable-model-invocation: false
---

# Translate AI Guidance to English

## Purpose

Convert Japanese AI guidance into English that an AI coding agent can execute reliably.
Do not perform literal translation. Rewrite the source as clear, actionable operational guidance.

Use this skill for:

- Japanese prompts
- Cursor Rules (`.mdc`)
- Japanese rule drafts that should become Cursor `.mdc` files
- AI agent rules
- Cursor Agent Skills (`SKILL.md`)
- System-prompt-like instructions
- Coding standards intended for AI execution

## Core Rule

Treat Japanese as the source of intent and English as the execution format.

The output MUST preserve the original intent, reduce ambiguity, and avoid adding new requirements.

## Workflow

1. Identify the source type:
   - Prompt
   - Cursor Rule
   - Agent Skill
   - Coding standard
   - General AI instruction
2. Normalize the Japanese source:
   - Split long sentences into one instruction per line.
   - Replace vague terms such as `なるべく`, `いい感じに`, `適切に`, and `必要に応じて` with concrete conditions.
   - Mark each instruction as `MUST`, `SHOULD`, `MAY`, or `NEVER`.
3. Convert to English:
   - Use concise imperative English.
   - Prefer standard technical terms.
   - Keep product names, file paths, class names, APIs, and code identifiers unchanged.
   - Preserve Japanese response-language requirements when the user-facing output must stay Japanese.
4. Validate meaning:
   - Compare the English version against the Japanese source.
   - List any assumptions.
   - List anything that needs human confirmation.
5. Review the converted output:
   - Confirm the output is useful as English AI guidance, regardless of source type.
   - Apply source-type checks only when the input is a Cursor Rule, Agent Skill, coding standard, or another specialized format.
   - Do not make Skill-specific or `.mdc`-specific checks the default for normal prompts.
6. Run a sample execution check when possible:
   - Apply the English guidance to one small representative input.
   - Confirm that the expected behavior is clear without reading the Japanese source.
7. Provide a Japanese back-translation summary:
   - Summarize what the English output means in Japanese.
   - Highlight any meaning changes or unresolved ambiguity.

## Output Format

Use this format unless the user requests a different one:

```markdown
## English Version

[Converted English guidance]

## Assumptions

- [Assumption, or "None"]

## Human Confirmation Needed

- [Item, or "None"]

## Japanese Back-Translation Summary

[Short Japanese summary of the English version]

## Quality Gate

- [ ] Intent is preserved
- [ ] Ambiguous Japanese expressions are made concrete
- [ ] No new requirements were added
- [ ] Technical terms are standard English
- [ ] User-facing Japanese language rules are preserved when needed
```

## Post-Conversion Review

After every conversion, review the output from the base purpose first:

- Intent is preserved from the Japanese source.
- Ambiguous Japanese expressions are converted into concrete conditions.
- The English guidance is actionable without reading the Japanese source.
- No new requirements, tools, dependencies, or workflows were added.
- Assumptions and human confirmation items are listed.

Then apply only the relevant optional review:

- For normal prompts: check role, task, context, constraints, and output format.
- For Cursor `.mdc` rules: check frontmatter, activation mode, globs, filename, and rule scope.
- For `SKILL.md` files: check skill metadata, trigger clarity, workflow executability, and linked references.
- For coding standards: check file scope, required behavior, prohibited behavior, and examples.

## Cursor Rule Conversion

When converting Japanese guidance into Cursor `.mdc` rules:

1. Decide whether the source should become one `.mdc` file or multiple files.
2. Choose the activation mode:
   - `alwaysApply: true` for universal rules only.
   - `globs` for language-specific or path-specific rules.
   - `description` without `globs` for agent-requested situational rules.
   - no `description` and no `globs` only for manual rules.
3. Propose a lowercase, hyphen-separated filename ending in `.mdc`.
4. Write a short `description` that states what the rule covers and when it applies.
5. Keep each rule focused on one concern and preferably under 50 lines.
6. Include short examples only when they reduce ambiguity.

Recommended structure:

```markdown
---
description: [What this rule enforces and when it applies]
globs: "[file pattern, if scoped]"
alwaysApply: false
---

# [Rule Title]

- MUST ...
- SHOULD ...
- NEVER ...
```

When the user asks for `.mdc` optimization, output:

```markdown
## Recommended Rule Files

| File | Activation | Purpose |
| --- | --- | --- |
| `.cursor/rules/[name].mdc` | [always / globs / agent-requested / manual] | [purpose] |

## MDC Files

### `.cursor/rules/[name].mdc`

\```markdown
---
description: [description]
globs: "[pattern]"
alwaysApply: false
---

# [Title]

- MUST ...
\```

## Human Confirmation Needed

- [Scope, glob, or naming question, or "None"]
```

## Skill Conversion

When converting a `SKILL.md` file:

- Preserve YAML frontmatter fields that already exist.
- Translate `name` only if it is Japanese or unclear; use lowercase letters, numbers, and hyphens.
- Rewrite `description` in third person and include trigger scenarios.
- Keep `SKILL.md` under 500 lines.
- Move detailed examples or long checklists into one-level-deep reference files.
- Do not invent scripts, tools, or dependencies unless the source explicitly requires them.

## Quality References

- For stricter validation, see [QUALITY_GATE.md](QUALITY_GATE.md).
- For `.mdc` rule design, see [MDC_OPTIMIZATION.md](MDC_OPTIMIZATION.md).
- For examples, see [examples.md](examples.md).
