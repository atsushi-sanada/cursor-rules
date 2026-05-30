# Quality Gate

Use this checklist before accepting an English conversion.

## 1. Intent Preservation

- The English version keeps the original purpose.
- The English version does not add requirements that were not present or directly implied.
- The English version does not remove constraints, exceptions, or priority rules.
- The English version keeps required output language rules, such as "respond in Japanese".

## 2. Ambiguity Reduction

Rewrite vague Japanese into concrete conditions.

| Japanese Pattern | Weak English | Better English |
| --- | --- | --- |
| なるべく | Try to | SHOULD, unless [condition] |
| いい感じに | Make it good | Define the target quality |
| 適切に | Properly | Specify the exact action |
| 必要に応じて | If needed | Define when it is needed |
| 〜しないように | Avoid | NEVER, or state the allowed alternative |

## 3. Instruction Strength

Use these terms consistently:

- `MUST`: Required behavior.
- `SHOULD`: Default behavior with valid exceptions.
- `MAY`: Optional behavior.
- `NEVER`: Prohibited behavior.

Do not mix `must`, `required`, `mandatory`, and `always` for the same meaning in one rule.

## 4. AI Execution Fit

The English output is ready when another agent can answer these questions without guessing:

- What task should I perform?
- What input should I use?
- What output format should I produce?
- What constraints must I obey?
- What should I do if information is missing?
- What should I never change?

## 4.5. Post-Conversion Review

Review every conversion as English AI guidance first.

- The review starts from the base purpose: converting Japanese intent into executable English guidance.
- Source-type checks are optional add-ons, not the default review frame.
- Normal prompts are not judged by Cursor Rule or Agent Skill requirements.
- Cursor Rule checks are applied only when `.mdc` output is requested or clearly implied.
- Agent Skill checks are applied only when `SKILL.md` output is requested or clearly implied.

## 5. Source-Type Checks

### Prompt

- The role, task, context, constraints, and output format are explicit.
- The prompt separates source text from instructions.
- The prompt tells the model what to do when information is missing.

### Cursor Rule

- The `.mdc` frontmatter is valid.
- `.mdc` Project Rules keep frontmatter at the top of the file.
- `.mdc` Project Rules do not add a filename or title line before the opening `---`.
- User Rules or other plain Markdown rules use a readable rule identifier on the first line.
- `alwaysApply` is used only for universal guidance.
- `globs` are used for file-specific rules.
- `globs` values are quoted, for example `"**/*.cs"`.
- Agent-requested rules have a specific `description` and no `globs`.
- Manual rules omit `description` and `globs` unless the user explicitly wants discoverability.
- The rule is scoped to one concern.
- Large Japanese sources are split into multiple `.mdc` files instead of copied into one large rule.
- Filenames are lowercase, hyphen-separated, and end with `.mdc`.
- Examples are short and directly useful.

### MDC Optimization

- The output includes a recommended file list.
- Each recommended file has an activation mode.
- Each activation mode has a reason.
- The output lists human confirmation items for unclear scope, glob patterns, or always-on behavior.
- The generated body uses `MUST`, `SHOULD`, `MAY`, and `NEVER` consistently.

### Agent Skill

- `name` is lowercase and hyphen-separated.
- `description` states both what the skill does and when to use it.
- `SKILL.md` contains the essential workflow only.
- Optional details are moved to one-level-deep reference files.
- The skill does not require tools or scripts that are not documented.
- The converted skill is reviewed after generation.
- The review confirms metadata validity, trigger clarity, workflow executability, linked references, and unresolved confirmation items.

## 6. Back-Translation Review

After conversion, summarize the English meaning in Japanese.

Flag the result as `Needs human confirmation` if:

- The Japanese source contains subjective quality terms.
- Multiple interpretations are valid.
- The English output changed priority, scope, or exceptions.
- The source uses domain-specific terms that may require team vocabulary.

## 7. Final Acceptance

Accept the conversion only if all items are true:

- [ ] The output is actionable.
- [ ] The output is concise.
- [ ] The output uses consistent terms.
- [ ] The output preserves Japanese user-facing requirements when needed.
- [ ] The output includes assumptions and human confirmation items.
- [ ] If `.mdc` files are requested, the output includes file paths, frontmatter, activation modes, and split rationale.
- [ ] If a `SKILL.md` file is requested, the output includes a post-conversion review result.
