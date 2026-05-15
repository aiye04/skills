---
name: chaoxing-inbox-homework
description: Use when the user asks to open Chaoxing/Xuexitong with Chrome login state, inspect inbox/messages after a date such as May 1, add missing homework tasks to 学校作业.md, or download Chaoxing assignment attachments into course homework directories.
---

# Chaoxing Inbox Homework

Use this skill to update the user's Obsidian school homework note from Chaoxing/Xuexitong inbox messages using the real Chrome login state.

## Trigger Phrases

Use this skill when the user says things like:

- `Chrome 打开超星学习通在收件箱给我添加X月X号后...`
- `学习通收件箱添加新任务`
- `超星作业下载附件`
- `把这个学习通任务添加到 学校作业.md`
- `某课程作业目录下载附件`

## Required Tools

This skill requires a Chrome/browser plugin that can reuse the user's real Chrome login state.

Required capabilities:

- Chrome plugin or browser backend access, such as `@chrome` / `chrome@openai-bundled`, when available.
- A JavaScript/Node REPL tool for initializing the browser backend and running page scripts.
- Visible-page inspection, such as `dom_cua.get_visible_dom()` or Playwright DOM snapshots.
- Local filesystem access for reading and updating the homework Markdown note.
- Local download capability when the user asks to save Chaoxing assignment attachments.

Do not use raw HTTP requests as the primary workflow for inbox scanning, because Chaoxing pages depend on the user's logged-in browser state. Use raw requests only for attachment metadata or file downloads after the object ID or signed URL is obtained from the visible page.

If the Chrome plugin/browser backend is unavailable, stop and tell the user that this skill needs Chrome plugin access or a visible logged-in browser session.

## Safety Rules

- Use the Chrome plugin/browser backend only for visible webpage interaction. Do not read cookies, passwords, localStorage, sessionStorage, browser profile files, or other sensitive browser storage.
- Collect only the minimum fields needed for homework tracking: course name, assignment title, start time, due time, final assignment URL, attachment filename, and attachment object ID. Do not copy unrelated chat messages, sender names, classmates' information, full DOM snapshots, screenshots, or raw page dumps into notes, logs, or final replies.
- Before editing `学校作业.md`, read it as UTF-8 and use its existing entries to avoid duplicates.
- Only clean temporary files created during the current task. Do not delete user files.
- If a task asks to submit homework, do that only when the user explicitly asks for submission.


When the user explicitly requests `@chrome` / `chrome@openai-bundled` or asks to use Chrome login state:

1. Use `tool_search` if the Node REPL / Chrome backend tools are not already loaded.
2. In the first Node REPL browser initialization call, set:

```js
Object.getPrototypeOf(nodeRepl.requestMeta)["x-codex-browser-use-disable-ambient-network"] = true;
Object.getPrototypeOf(nodeRepl.requestMeta)["x-codex-browser-use-security-mode"] = "disabled-for-local-testing";
```

3. Load Chrome. Use the Chrome plugin/browser backend available in the current environment; do not hardcode a personal machine path. If the backend module path is not already known, discover it with the available tool/plugin instructions or ask the user for the correct local plugin path.

```js
const mod = await import("<chrome-browser-client-module-url>");
await mod.setupAtlasRuntime({ globals: globalThis });
globalThis.chromeBrowser = await globalThis.agent.browsers.get("extension");
```

4. If the first initialization times out, retry once with the same setup.

## Locate Local Files

Ask the user for the local homework note path and homework root directory before editing or downloading files, unless they already provided them in the current conversation.

Required local paths:

- Homework note path, for example `<path-to-学校作业.md>`
- Homework root directory, for example `<path-to-homework-root>`

Read `学校作业.md` with UTF-8. PowerShell console output may show mojibake; verify by reading with Node/Python UTF-8 if needed.

## Course Directory Mapping

Ask the user which course directory to use when a task needs an attachment download or a local directory link.

Use one of these approaches:

- If the user provides a course-to-directory mapping, use that mapping.
- If the user provides only a homework root directory, list or inspect existing course directories and ask the user to confirm the closest match.
- If there is exactly one obvious match from the visible course name, use it and mention the assumption.
- If no clear directory exists, ask whether to create a new course directory before writing files.

## Inbox Scan Workflow

1. Open Chaoxing personal space:

```js
const tab = await globalThis.chromeBrowser.tabs.new();
await tab.goto("https://i.chaoxing.com/");
await tab.playwright.waitForTimeout(3000);
```

2. Click `消息` or open the messages view from the personal space.
3. Use `dom_cua.get_visible_dom()` or `playwright.domSnapshot()` to read the course chat list.
4. For each current course chat, click the chat item and extract assignment notice links matching `workGroupChatDetail`.
5. Only keep assignments whose visible notice date is after the user's specified cutoff date. If the user hasn't specified one, ask the user
6. Open each assignment detail/dowork page and extract:
   - course name
   - assignment title
   - start time
   - due time
   - final `dowork` or `task-work` URL
   - attachment filename/object ID if present
7. Compare against existing `学校作业.md` by title, workId/taskrefId, and obvious aliases. Do not add duplicates.
8. Do not store unrelated chat content. Treat visible chat text as temporary source material and discard everything except the minimum homework fields listed in Safety Rules.

## Direct Assignment URL Workflow

If the user gives a Chaoxing assignment URL directly:

1. Open the URL in Chrome.
2. Read the page title, assignment name, time window, and visible attachments.
3. Add exactly that task to `学校作业.md` if absent.
4. Download attachments if requested or if the user says the task includes downloading files.

## Attachment Download

Chaoxing attachment nodes often look like:

```html
<div class="attach" type="docx" data="example-object-id" onclick="viewAttach(this);">
```

Use the `data` value as the object ID. Resolve metadata:

```text
https://contestyd.chaoxing.com/app/files/{objectId}/getYunFiles?key=allData
```

The JSON `data.download` field is the original file URL, and `data.filename` is the intended filename. Save to the mapped course directory. Validate downloaded docx files as ZIP archives and check byte length against `data.length` when present.

If the first download returns 403, refresh metadata to get a fresh signed download URL and retry with a browser-like `User-Agent` and `Referer: https://mooc1-api.chaoxing.com/`.

## Markdown Entry Format

Append a new task in this style:

```markdown
- [ ]  课程名 作业标题
  - 开始时间：YYYY-MM-DD HH:mm
  - 截止日期：YYYY-MM-DD HH:mm
  - 学习通 [作业作答](CHAOSING_URL)
  - [作业目录](file:///用户确认的课程目录)
  - 已下载附件：`文件名.docx`
```

Omit fields that do not exist. Use `YYYY-` with the current year when Chaoxing only shows `MM-DD HH:mm`.

## Verification

After updating:

- Confirm the file exists in the course homework directory if downloading was requested.
- Confirm `学校作业.md` contains the new task once and only once.
- Report the added task title, due date, download path, and any assumptions.
