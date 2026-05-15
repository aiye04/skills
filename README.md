# skills

Personal agent skills for Codex and other agents that support the `SKILL.md` format.

This repository follows the common skills repository layout:

```text
skills/
└── docx-experiment-report-template-fill-preserve/
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
Copy-Item -Recurse .\skills\docx-experiment-report-template-fill-preserve "$env:USERPROFILE\.codex\skills\"
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

## 中文说明

这是我的个人 Agent Skills 仓库，主要给 Codex 使用。

当前包含：

- `docx-experiment-report-template-fill-preserve`：用于补全 Word 实验报告、课程报告、需求文档等 `.docx` 模板。重点是只填写空白和占位符，并尽量保留原始格式。

推荐目录结构：

```text
skills/
└── docx-experiment-report-template-fill-preserve/
    └── SKILL.md
```

安装后需要重启 Codex 才能识别新 Skill。
