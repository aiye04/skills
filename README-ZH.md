# skills

个人 Agent Skills 仓库，主要用于 Codex，也兼容其他支持 `SKILL.md` 格式的 Agent。

语言：[English](./README.md) | 简体中文

本仓库将每个个人 Skill 直接放在仓库根目录：

```text
docx-experiment-report-template-fill-preserve/
└── SKILL.md
```

## 已包含的 Skills

### `docx-experiment-report-template-fill-preserve`

用于填写和更新 Word 文档模板，并尽量保留已有内容和格式。

适用 `.docx` 模板类型：

- 实验报告
- 课程报告
- 需求文档
- 表单类文档

适合这些提示：

- “只填空白处”
- “有内容的地方不用动”
- “不要破坏格式”
- “补全这个实验报告模板”
- “替换 XXX 或 待填写 等占位符”

它会把原始文档结构、标题、表格、样式、页眉页脚和页面布局视为需要保留的内容。

## 安装

安装仓库里的所有 Skills：

```bash
npx skills add aiye04/skills
```

只安装这个 Skill：

```bash
npx skills add aiye04/skills --skill docx-experiment-report-template-fill-preserve
```

全局安装到 Codex：

```bash
npx skills add aiye04/skills --skill docx-experiment-report-template-fill-preserve -g -a codex
```

也可以手动复制：

```powershell
Copy-Item -Recurse .\docx-experiment-report-template-fill-preserve "$env:USERPROFILE\.codex\skills\"
```

安装或复制后，需要重启 Codex 才能识别新 Skill。

## Skill 格式

每个 Skill 是一个包含 `SKILL.md` 的目录，文件开头需要 YAML frontmatter：

```markdown
---
name: example-skill
description: 这个 Skill 的作用，以及什么时候使用它。
---

# Example Skill

给 Agent 的具体说明。
```
