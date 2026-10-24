# Adaptive Research Tutor — 自适应科研与工程导师

English first; a complete Chinese translation follows below.
英文在前，完整中文说明见下文。

A reusable Codex Skill for learning the principles behind the work you are already doing.

The central idea is context-adaptive teaching: use the task, conversation, implementation,
and evidence actually visible to the tutor to build a small learning path. Explain one
main concept at a time, answer follow-up questions for as long as needed, and let the
main task continue at its own pace.

This is an instruction-only Skill. It does not create a background process or provide
continuous cross-chat synchronization.

## What it does

| Capability | Behavior |
| --- | --- |
| Context adaptation | Identify relevant domains and prerequisites from accessible work; no discipline whitelist. |
| Interactive teaching | Stay on the current concept for “I do not understand,” examples, or deeper explanations. |
| Progressive learning | Follow concept dependencies while allowing explicit skips, detours, and returns. |
| Independent pace | Teaching does not require authorized development work to wait for understanding. |
| Progress recovery | With permission, save the active concept, questions, queue, and evidence versions in project learning files. |
| Evidence boundaries | Separate verified behavior, documentation, inference, and untested assumptions. |

A brief question in a development chat gets a brief answer. Ordinary coding or testing
requests alone do not start a course.

## Repository map

- [`adaptive-research-tutor/SKILL.md`](adaptive-research-tutor/SKILL.md) — entry point, triggers, boundaries, and workflow.
- [`agents/openai.yaml`](adaptive-research-tutor/agents/openai.yaml) — Skill display metadata.
- [`teaching-protocol.md`](adaptive-research-tutor/references/teaching-protocol.md) — concept states and follow-up rules.
- [`context-and-checkpoints.md`](adaptive-research-tutor/references/context-and-checkpoints.md) — accessible context, version changes, and optional main-task checkpoints.
- [`progress-schema.md`](adaptive-research-tutor/references/progress-schema.md) — lightweight learning records and recovery.
- [`evaluation-cases.md`](adaptive-research-tutor/references/evaluation-cases.md) — behavioral evaluation scenarios.

## First use

Copy the complete `adaptive-research-tutor` folder into `$CODEX_HOME/skills/`, or
`~/.codex/skills/` when `CODEX_HOME` is unset. Keep its internal directory structure.
Compare an existing installation before replacing it.

In a teaching chat that can access the target project, send:

```text
$adaptive-research-tutor Start teaching the principles behind my current task.
Task: describe what you are working on.
Project: provide your actual project path.
```

If the name is not yet discovered in your client, provide the installed `SKILL.md`
path and ask Codex to read and use it.

When available, use an associated Side Chat or a separate chat with access to the
same project. Shared files and checkpoints provide snapshots; they do not imply
live access to the main chat.

## Context adaptation and optional background

You do not need to fill in a personal profile before starting. The tutor uses the
context actually available in the starting conversation and relevant project
materials, then adjusts to your feedback. Inferred familiarity is tentative; project
experience alone does not prove mastery.

This repository contains no prefilled author or learner profile. You may optionally
provide your own background in your teaching conversation:

```text
My background (optional):
- Knowledge or tools I already know:
- Foundations I would like to strengthen:
- The question I most want to understand:
- Preferred explanation style and depth:
```

Leave any field blank. Personal background belongs in your conversation or, with
permission, your own project learning records. Do not add it to the public Skill.
The example `learner_model.stated` and `learner_model.observed` fields start empty.

## Follow-ups, synchronization, and recovery

- “I do not understand,” “give an example,” or “explain differently”: stay on the current concept.
- “Understood”: record understanding; “understood, continue”: move to the next concept.
- “Skip this for now”: move on while preserving the unresolved question.
- “Synchronize the latest main-task progress”: read accessible changes and add relevant concepts without interrupting the current lesson.
- “Pause teaching and save this question”: save within existing file permissions, or provide a conversation-only recovery summary.
- In a new chat, invoke `$adaptive-research-tutor` to resume and provide the actual progress-file location.

Use the project's existing learning structure when present; otherwise `docs/learning/`
is an optional convention. Source, configuration, data, and experiment files remain
read-only to the tutor. Main-task checkpoints and tutor progress have separate writers.

## Optional main-task integration

The [checkpoint reference](adaptive-research-tutor/references/context-and-checkpoints.md)
contains an optional `AGENTS.md` snippet. Enable it explicitly in the main development
chat to write concise checkpoints at meaningful milestones. Installing this Skill does
not modify `AGENTS.md`.

## Evaluation and limits

The [evaluation scenarios](adaptive-research-tutor/references/evaluation-cases.md)
cover three different domains, repeated follow-ups, fresh-context recovery, main-task
version changes, negative triggers, and missing context. Development exercises used
synthetic projects and independent tutor sessions. They do not establish automatic
Skill discovery in every client or continuous concurrency between two user chats.

The publication package is checked for valid Skill frontmatter, relative references,
and YAML examples. Personal learning records, local validation dependencies, and test
session transcripts are not bundled. See the [bilingual changelog](CHANGELOG.md).

If project or main-chat access is unavailable, the tutor should say so and work from
what the user supplies. If persistence is unavailable, progress remains in the current
conversation unless the user carries a recovery summary forward.

## License

The project-authored Skill instructions and documentation are released under the
[MIT License](LICENSE). Copyright (c) 2026 Catmachine.

## 中文说明

一个可复用的 Codex Skill：围绕用户当前的科研、工程或其他复杂任务，逐点讲解实际工作背后的原理，支持反复追问和学习位置恢复。

### 功能

- 从可访问的任务、代码、文档和实验记录中识别知识领域，不预设学科范围。
- 每次讲清一个主要知识点，可以持续追问、换一种讲法、自由跳转。
- 主任务继续执行，教学按学习者自己的节奏推进；导师默认只读工程文件。
- 获得授权后，在项目学习资料中保存位置、疑问和知识队列。
- 区分实现事实、文档规定、推断和假设，不编造已完成的测试或同步结果。

### 仓库结构

```text
.
├── README.md
├── .gitignore
└── adaptive-research-tutor/
    ├── SKILL.md
    ├── agents/
    │   └── openai.yaml
    └── references/
        ├── teaching-protocol.md
        ├── context-and-checkpoints.md
        ├── progress-schema.md
        └── evaluation-cases.md
```

### 安装与调用

将整个 `adaptive-research-tutor` 文件夹复制到个人 Skill 目录 `$CODEX_HOME/skills/`；没有设置 `CODEX_HOME` 时使用 `~/.codex/skills/`。保留文件夹内的完整结构，已有同名 Skill 时先比较内容，避免直接覆盖。

在能够读取目标项目的教学聊天中发送：

```text
$adaptive-research-tutor 开始教学，解释当前任务中的关键原理。
主任务：填写你正在完成的任务。
项目位置：填写你的项目实际路径。
```

如客户端尚未识别名称，也可以直接提供安装后的 `SKILL.md` 路径，请 Codex 读取并使用。

### 上下文自适应与可选知识背景

无需预先填写知识背景。导师会结合启动会话中实际可见的任务目标、对话、相关文件和学习反馈调整讲解。手动填写背景是可选补充；由上下文推测的能力只能作为待确认信息，不直接认定用户已经掌握相关知识。

本仓库不预填作者或任何特定用户的知识背景。每位用户可以在自己的教学聊天中填写以下内容，导师会据此调整解释深度；也可以全部留空，直接开始教学。

```text
我的知识背景（可选）：
- 已掌握的知识或工具：填写你的情况，或留空。
- 希望补充的基础：填写你的情况，或留空。
- 当前最想理解的问题：填写你的问题，或留空。
- 讲解偏好和期望深度：填写你的偏好，或留空。

请仅根据我提供的信息和本次会话反馈调整教学，不要假设我已掌握未声明的知识。
```

不需要把个人信息写入 `SKILL.md` 或提交到这个仓库。背景默认用于当前会话；需要跨会话保存时，请明确允许导师写入你自己的项目学习资料。进度格式中的 `learner_model.stated` 和 `learner_model.observed` 默认都是空列表，前者记录用户自述，后者记录会话理解反馈。

### 交互、同步与恢复

- “没懂”“举个例子”“换一种方式讲解”：继续当前知识点。
- “懂了”：记录已理解；“懂了，继续”：记录后进入下一个点。
- “先跳过，讲另一个点”：保留疑问并跳转，不强制测验或练习。
- “同步主任务的最新进度”：读取当前可访问的检查点或相关文件，新知识加入队列，不抢占正在学习的位置。
- “暂停教学，请保存当前卡点”：在已有写入授权范围内保存；没有写入权限时提供会话内摘要。
- 新聊天中使用 `$adaptive-research-tutor 继续学习`，提供实际学习记录路径，可从上次位置恢复。

优先沿用项目已有的学习目录；没有约定时可使用 `docs/learning/`。背景、学习记录和用户笔记属于各自的项目，不随公共 Skill 发布。

### 可选主任务检查点

[上下文与检查点说明](adaptive-research-tutor/references/context-and-checkpoints.md)包含可选 `AGENTS.md` 集成片段。用户明确选择启用后，主开发会话可在重要里程碑生成简短检查点，教学会话按需读取。安装本 Skill 不会自动修改 `AGENTS.md`。

### 能力边界与验收

这是纯指令 Skill，没有后台进程、持续监听或实时跨聊天同步能力。持续独立教学可以使用客户端可用的关联 Side Chat，或能够读取同一项目的独立聊天；具体能力取决于工具和权限。无法读取主任务或保存文件时，导师应说明限制。

[验收场景](adaptive-research-tutor/references/evaluation-cases.md)涵盖跨领域适应、多轮追问、进度恢复、主任务变化、误触发及上下文缺失。该文件提供复测方法；实际学习记录、本机验证依赖和测试会话输出不包含在本仓库中。

### 许可证

本项目原创的 Skill 指令与文档采用 [MIT 许可证](LICENSE)。Copyright (c) 2026 Catmachine。
