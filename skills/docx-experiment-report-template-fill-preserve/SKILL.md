---
name: docx-experiment-report-template-fill-preserve
description: Fill and update Word document templates while preserving existing content and formatting. Use when the user provides a .docx template, experiment report, requirements document, course report, or form-like document and asks to fill blanks, replace placeholders, add required content, complete the project/report, or keep existing formatted content unchanged. Trigger especially for Chinese prompts such as “只填空白处”, “有内容的地方不用动”, “不要破坏格式”, “按模板补全”, “实验报告”, “需求文档”, or “自己完成并填写”.
---

# DOCX Experiment Report Template Fill Preserve

## Core behavior

Use template-fill mode by default.

Preserve the original document as the source of truth. Fill only blanks, placeholders, obviously unfinished areas, and locations explicitly requested by the user. Do not rewrite, reorganize, beautify, or reformat existing complete content unless the user directly asks for that change.

Keep the original title hierarchy, numbering, tables, paragraph styles, fonts, spacing, headers, footers, page layout, and existing section order.

When editing `.docx`, prefer targeted text replacement and insertion. Do not rebuild the whole document unless there is no safer option. When adding text, copy the style of the neighboring paragraph, table cell, or same-level heading.

## Decision rules

Classify every editable area before writing.

1. Existing content: leave unchanged.
2. Placeholder or blank content: fill when enough information exists or can be reasonably generated.
3. User-specified changes: modify only the requested location.
4. Teacher/evaluator fields: usually leave blank unless the user explicitly asks to fill them.
5. Personal/course identity fields: ask the user when missing.
6. Evidence fields, such as screenshots, real run results, logs, exact code output, grades, signatures, or comments from a teacher: do not fabricate.

Treat strings such as `XXX`, `XXXX`, `待填写`, blank table cells, empty paragraphs under a heading, `年 月 日`, and obvious template gaps as fill targets.

## Missing-information policy

Do not silently mark important missing facts as `【待补充】` when the user expects a finished document.

Ask the user first when required factual information is missing and cannot be safely inferred, especially:

- student name
- student ID
- class
- major
- instructor
- course name
- experiment location
- submission date
- exact project topic when the document depends on it
- required code, screenshots, logs, or measured results

Keep questions minimal and grouped. Ask only for information that blocks the document from being reasonably completed.

If the missing item is not blocking, continue the task and mark the location as `【需用户确认】` or `【由用户补充】` only for the small parts the user must personally verify or provide.

When the user says to “全程自己做”, “自己完成”, “能填就填”, or similar, generate reasonable content for general narrative sections, but still ask for personal identity facts and real evidence that must not be invented.

## Autofill policy

Generate content without asking when it is generic, document-derived, or safely inferable from the project context.

Usually autofill:

- experiment purpose
- experiment principle
- experiment content and requirements
- software/hardware environment, if the topic implies common tools
- procedure description
- result analysis written at a non-fabricated level
- common problem analysis
- experiment summary
- requirement-document background, scope, functional requirements, non-functional requirements, acceptance criteria, risks, and test points

Avoid claiming that code ran, screenshots were captured, tests passed, or data was measured unless the user provides evidence. Use wording such as “运行后应显示……”, “截图位置由用户补充”, or “结果以实际运行为准” when necessary.

## Workflow

1. Inspect the document structure.
2. Identify fixed content, blanks, placeholders, and user-requested edit areas.
3. Identify missing blocking facts.
4. If blocking facts are missing, ask concise questions before editing.
5. If enough information exists, edit the file directly.
6. Preserve formatting by copying nearby styles for inserted text.
7. Save a new `.docx` file instead of overwriting the original.
8. Return the edited file and a short change list.

## Experiment report rules

For student experiment-report templates:

- Fill general sections from the experiment topic and available context.
- Leave `实验成绩`, `评语`, and `教师签名` blank unless the user explicitly requests otherwise.
- Keep `其他成员：无` unchanged when already present.
- Do not invent screenshots, exact console output, score, teacher comments, or real execution logs.
- If no abnormal issue is provided, write a realistic non-fabricated statement such as “本次实验未出现影响完成的异常问题；如后续运行中出现报错，可根据错误提示检查环境配置、路径和依赖版本。”
- If the template requires deleting red instruction text and red text cannot be reliably identified, remove only clearly recognized template instructions or tell the user which parts need manual review.

## Requirements document rules

For requirement documents:

- Keep existing completed sections unchanged.
- Fill empty or placeholder sections with complete, practical content.
- Add missing details only where the template asks for them.
- Prefer structured content: background, objectives, users, roles, business process, functional requirements, non-functional requirements, data requirements, interface requirements, acceptance criteria, risks, and open questions.
- Mark business facts that require product-owner confirmation as `【需确认】`.
- Do not invent real client names, legal commitments, production metrics, private data, or signed-off decisions.

## Output requirements

When done, provide:

1. The new edited document file.
2. A concise completion note.
3. A change list that names the filled or modified sections.
4. A short list of items requiring user confirmation or manual insertion, if any.

Use this style for the completion note:

```text
已完成模板补全，并尽量保持原文档格式。
已填写/修改：……
需要你确认或手动补充：……
```

## Prohibited behavior

Do not:

- rewrite the entire document
- change layout for aesthetics
- delete existing content without explicit permission
- change table structure, column widths, borders, or colors unnecessarily
- change heading hierarchy or numbering
- fabricate personal information
- fabricate real execution results, screenshots, grades, signatures, or teacher comments
- treat complete existing content as text to optimize
- leave important missing facts as placeholders without asking when they block completion
