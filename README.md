<h1 align="center">🧰 Alier-skill</h1>
<p align="center"><strong>个人 AI Skills 合集</strong> — 一次配置，多工具复用</p>

<p align="center">
  <img src="https://img.shields.io/github/license/aiye04/Alier-skill" alt="License">
  <img src="https://img.shields.io/badge/skills-1-blue" alt="Skills Count">
  <img src="https://img.shields.io/badge/tools-Claude%20Code%20%7C%20Codex%20%7C%20OpenClaw-orange" alt="Compatible Tools">
</p>

---

## 什么是 Skill？

Skill 是一组预定义的指令模板，让 AI 编码助手按特定流程和规则执行任务。一个 Skill 就是一个包含 `SKILL.md` 的文件夹，可以被 Claude Code、Codex、OpenClaw 等工具直接加载。

## 已有 Skills

| Skill | 触发场景 |
|-------|---------|
| [docx-experiment-report-template-fill-preserve](./skills/docx-experiment-report-template-fill-preserve/) | 给定 .docx 模板 → 识别占位/空白处 → 按规则自动填充，保留原有格式和已有内容 |

## 快速开始

```bash
# 1. 克隆仓库
git clone https://github.com/aiye04/Alier-skill.git

# 2. 安装到对应工具
cp -r Alier-skill/skills/<skill名> ~/.agents/skills/

# 3. 在对话中直接描述需求即可触发
```

## 目录结构

```
Alier-skill/
├── skills/            # 所有 Skill
│   └── <skill名>/
│       └── SKILL.md   # Skill 定义文件
├── .gitignore
├── LICENSE
└── README.md
```

## 贡献

欢迎提 Issue / PR 分享你自己的 Skills。
