# AGENTS.md

> 本文件遵循 [AGENTS.md](https://agents.md) 约定，供自动读取本仓库的 AI 编程助手识别本项目用途与上手方式。

## 这个仓库是什么

**Nuwa（女娲）** 是一个**提示词架构师**：你给它一句"我想要一个什么样的 agent"，它就为你**编译出一份健壮、可直接部署的单 agent 系统提示词**——自带安全护栏、闭环、诚实边界、反注入等模块，并按风险分 **Lite / Standard / Fortress** 三档。

仓库内容：
- `nuwa-7.0.xml` — 女娲内核（锻造方法论：六步编译流程 + 全部护栏）。
- `START_HERE.md` — 给 AI 助手的落地指令（如何把女娲安装进用户项目并启动）。
- `templates/`、`examples/` — 模板与示例。

## 若你是 AI 助手

请按 **[`START_HERE.md`](./START_HERE.md)** 的分步骤，帮用户把女娲落地：

1. 在用户项目里新建 `nuwa-base/` 文件夹；
2. 把内核 `nuwa-7.0.xml` 复制进去，并按 `START_HERE.md` 里的模板生成 `nuwa-base/CLAUDE.md` 与 `nuwa-base/如何使用.md`；
3. 完成后**主动提醒用户**：新开一个对话、把工作目录设到 `nuwa-base/`，女娲即会加载启动；然后直接描述想要的 agent（名字 / 角色 / 职责 / 边界 / 风险）即可。

## 若你是人类用户

最省事的做法：把本项目交给你的 AI 助手，说一句"请按 START_HERE.md 帮我落地女娲"。它会建好 `nuwa-base/` 并告诉你怎么启动。也可以自己读 `START_HERE.md` 手动操作。
