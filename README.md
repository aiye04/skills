# skills

Personal agent skills for Codex and other agents that support the `SKILL.md` format.

Language: English | [简体中文](./README-ZH.md)

This repository keeps each personal skill at the repository root:

```text
chaoxing-inbox-homework/
└── SKILL.md
docx-experiment-report-template-fill-preserve/
└── SKILL.md
```

## Available Skills

### `docx-experiment-report-template-fill-preserve`

Fill and update Word document templates while preserving existing content and formatting.

Use it when working with `.docx` templates such as:

- experiment reports
- course reports
- requirements documents
- form-like documents

The skill is designed for prompts like:

- "only fill the blank parts"
- "do not change existing content"
- "preserve the original format"
- "complete this experiment report template"
- "replace placeholders such as XXX or 待填写"

It keeps the original document structure, headings, tables, styles, headers, footers, and page layout as the source of truth.

### `chaoxing-inbox-homework`

Update a local homework note from Chaoxing/Xuexitong inbox messages using the user's visible Chrome login state.

Use it when the user asks to:

- inspect Chaoxing inbox messages after a specified date
- add missing homework tasks to a local Markdown note
- download assignment attachments into a user-confirmed course directory

The skill asks for local paths and course directories at runtime, and it avoids reading or storing browser cookies, passwords, unrelated chat messages, or raw page dumps.

## Install

Install all skills from this repository:

```bash
npx skills add aiye04/skills
```

Install only this skill:

```bash
npx skills add aiye04/skills --skill docx-experiment-report-template-fill-preserve
```

Install globally for Codex:

```bash
npx skills add aiye04/skills --skill docx-experiment-report-template-fill-preserve -g -a codex
```

You can also copy the skill manually:

```powershell
Copy-Item -Recurse .\docx-experiment-report-template-fill-preserve "$env:USERPROFILE\.codex\skills\"
```

Restart Codex after installing or copying a new skill.

## Skill Format

Each skill is a directory containing a `SKILL.md` file with YAML frontmatter:

```markdown
---
name: example-skill
description: What the skill does and when to use it.
---

# Example Skill

Instructions for the agent.
```
