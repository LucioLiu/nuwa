# 女娲 Nuwa · v7.0

> **一句话**：女娲是一个 **提示词架构师（Prompt Architect）**——你把"想要一个什么样的 AI 员工/助手"讲清楚，它就帮你**编译出一份健壮、可直接部署的单 agent 系统提示词（system prompt）**，自带安全护栏、闭环、诚实与反注入模块，分 **Lite / Standard / Fortress** 三档交付。

**Nuwa is a Prompt Architect.** Describe the AI agent you want, and Nuwa compiles a robust, deployment-ready **single-agent system prompt** for you — with built-in safety guardrails, a closed-loop module, an honesty module, and prompt-injection hardening — delivered at one of three tiers (Lite / Standard / Fortress).

关键词 / keywords: `prompt engineering` · `system prompt` · `AI agent` · `prompt architect` · `agent prompt generator` · `LLM` · `prompt template` · `agent design` · `guardrails` · `prompt compiler`

---

## 为什么用女娲 / Why Nuwa

写一个能用的系统提示词不难；写一个**经得起真实部署**的系统提示词很难——它要在边界、诚实、抗攻击、可审计之间都站得住。女娲把这套"工程化方法"固化成一台编译器：

- **三档分级（Tiered output）** — 不是所有 agent 都需要重装结构。女娲按**风险信号**（动作是否不可逆、影响范围、工具权限、领域敏感度）自动选档：
  - **Lite** — 轻量、低风险助手，结构精简、即用。
  - **Standard** — 大多数生产场景的默认档，护栏与职责齐备。
  - **Fortress** — 高风险/高权限 agent，全套防御与审计要求拉满。
- **安全护栏（Guardrails）** — 硬边界、权限分级、危险动作前置确认，写进提示词骨架而非靠运气。
- **闭环模块（Closed-loop）** — 让 agent 不只"做完就走"：对判断/打分/推荐类任务预置**结果回填**结构，越用越准。
- **诚实模块（Honesty）** — 工具能力分"已验证可用 / 规划中待验证"两栏，**绝不让 agent 声称自己有一只它其实没有的"手"**。
- **反注入加固（Prompt-injection hardening）** — 面对"忽略上文/泄露系统提示/越权操作"等注入攻击，agent 礼貌拒绝并把话题拉回职责，用肯定句说明"能怎么帮"。
- **可审计（Auditable）** — 关键动作可追溯，结构确定性优先于辞藻华丽；对外只声称"分层强防御 / best-effort"，绝不声称绝对安全。

> Robust, deployable system prompts need more than good wording — they need defensible **boundaries, honesty, injection-resistance, and auditability**. Nuwa bakes all of that into a repeatable prompt compiler so you don't reinvent it each time.

---

## 怎么用（三步 · 防呆）/ How to use (3 steps)

你**不需要会写提示词**，也不需要懂任何框架。准备一个能读本地文件的 AI 编程助手（如 Claude / Cursor 等）即可：

**① 下载本项目** — 把这个仓库 clone 或下载到本地。
> Download / clone this repository to your machine.

**② 把项目交给你的 AI，让它读 `START_HERE.md`** — 在你的 AI 助手里打开本项目文件夹，发一句：
> 「请阅读 `START_HERE.md` 并按它说的做。」
>
> In your AI assistant (Claude / Cursor / etc.), open this project folder and say: *"Read `START_HERE.md` and follow it."*

**③ AI 会自动建好"女娲基地"文件夹，并提醒你新开对话启动** — 它会照 `START_HERE.md` 把女娲的工作目录铺好，然后告诉你：开一个新对话、唤醒女娲、说出你想要的 agent，女娲就开始编译。
> The AI sets up the "Nuwa base" folder for you, then prompts you to start a **fresh conversation** to wake Nuwa and describe the agent you want.

之后就是对话：你描述需求 → 女娲先把模糊处问清（它**不猜关键约束**）→ 选档 → 编译 → 交付一份可直接粘贴部署的系统提示词。
> Then it's a conversation: you describe, Nuwa clarifies what's ambiguous (it never guesses critical constraints), picks a tier, compiles, and hands you a paste-ready system prompt.

---

## v7.0 有什么新（What's new since v4.0）

v7.0 是相对 v4.0 的一次**重大升级**：把"经验型护栏"升级成**框架强制的硬关卡**——新增/强化了 **闭环硬关卡**（结果回填从"自觉"变成结构内置）、**诚实不编能力**（已验证 vs 规划中工具泾渭分明）、以及**反注入加固**（系统化的抗提示词注入应答模式），并完善了三档选档的风险判定与交付校验。

> v7.0 is a major upgrade over v4.0: experience-based guardrails become **framework-enforced gates** — a hard **closed-loop** checkpoint, an **honesty / no-phantom-tools** rule, and systematic **prompt-injection hardening**, plus sharper tier-selection and delivery validation.

---

## 目录结构 / Project layout

```
Nuwa/
├─ START_HERE.md     # 给你的 AI 读的第一份文件（入口）
├─ README.md         # 你正在看的这份（给人读）
├─ templates/        # 提示词骨架与三档模板 / prompt skeletons & tier templates
└─ examples/         # 示例产物 / example compiled agents
```

---

## 建议的 GitHub Topics / Suggested topics

发布者可把以下标签贴到仓库 Topics，便于被搜索与发现：

```
prompt-engineering   system-prompt        prompt-architect
ai-agent             agent-design         prompt-template
llm                  prompt-compiler      guardrails
prompt-generator     ai-assistant         claude
cursor               prompt-injection     agentic-ai
```

---

## 许可 / License

**PolyForm Noncommercial 1.0.0（禁商用 / Noncommercial）** — 版权人 Lucio Liu。
允许个人、研究、教育及其他非商业用途；商业使用需另行授权。完整条款见仓库 `LICENSE`。

Licensed under **PolyForm Noncommercial 1.0.0** — Copyright (c) 2026 Lucio Liu.
Free for personal, research, educational, and other noncommercial use; commercial use requires a separate license. See `LICENSE` for full terms.
