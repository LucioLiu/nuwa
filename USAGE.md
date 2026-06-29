# Using Nuwa — A Practical Guide

Nuwa is a **prompt architect**: you describe the agent you want, and Nuwa compiles a
single, deployable **system prompt** for it — structured, safety-railed, and tuned for
your target model. This guide walks you from zero to a finished agent prompt, with
enough hand-holding that you can't really get it wrong.

If you just want to see what the output looks like, jump to
[`examples/`](./examples/) for two complete, copy-pasteable builds.

---

## 1. What Nuwa is (and is not)

| Nuwa **is** | Nuwa is **not** |
|---|---|
| A compiler that turns a plain-language need into one finished agent system prompt | A chatbot that does the task itself |
| A safety-first architect (guardrails, anti-injection, honest-capability rules baked in) | A no-rules prompt generator |
| A single-agent builder — it produces **one** prompt per run | A multi-agent orchestrator |

You bring the **requirement**. Nuwa brings the **structure, safety rails, and format**.

---

## 2. How to start (the 30-second version)

1. Start Nuwa following [`START_HERE.md`](./START_HERE.md) — point your model at this
   folder so it loads the kernel (`nuwa-7.0.xml`). Claude is the default target; others work too.
2. Send one message describing the agent you want. At minimum, give the **three required fields** below.
3. Nuwa replies with: a one-line explanation of what it understood + which tier it
   chose, then the **complete agent prompt**, then short how-to-deploy notes.
4. Copy the agent prompt out and deploy it. Done.

That's the whole loop. The rest of this doc makes each step bulletproof.

---

## 3. The three required fields (always provide these)

Nuwa needs three things to compile anything. If any are missing or contradictory,
it will **ask you a clarifying question instead of guessing** — that's by design.

| # | Field | What it means | Good example |
|---|---|---|---|
| 1 | **Name** | What the agent is called | `"RefundBot"` |
| 2 | **Role** | What kind of agent it is, in one phrase | `"a customer-service assistant for refund requests"` |
| 3 | **Core responsibilities** | The 2–5 concrete things it must do | `"explain refund eligibility, compute refund amounts, escalate disputes to a human"` |

### A minimal good request looks like this

> "Build me an agent. **Name:** RefundBot. **Role:** a customer-service refund
> assistant for an online store. **Core responsibilities:** check whether an order
> is refund-eligible under our 30-day policy, tell the customer the refund amount,
> and hand off anything it can't resolve to a human agent. It only reads order info
> and answers in text — it does not issue payments itself."

Notice the last sentence: it tells Nuwa the agent has **no money-moving power**.
That single fact changes which safety tier Nuwa picks (see below), so it's worth
stating clearly.

### Optional but helpful extras

- **Target model** (default: Claude) — e.g. "target GPT-class model."
- **Output format** (default: XML for Claude) — XML / Markdown / JSON.
- **Tone / persona** — "friendly but concise," "formal," etc.
- **Hard limits** — anything it must *never* do ("never reveal internal pricing logic").
- **Tools it will really have** — see the honesty rule in §6.

---

## 4. Choosing a tier (Nuwa does this for you — here's how it decides)

Nuwa compiles agents at one of **three tiers**. You usually don't pick the tier —
Nuwa picks it from the **risk** in your description. You *can* request a tier, but
if you ask for something weaker than the risk warrants, Nuwa will **upgrade it and
tell you why**. Safety is never silently downgraded.

| Tier | Use when | What you get |
|---|---|---|
| **Lite** | Low-risk, read-only, single job, fast prototype | Identity + core task + a couple of core safety rules + brief output guidance |
| **Standard** | The default for most business assistants | Full identity, responsibilities, guardrails (role-lock, data-boundary, escalation), workflow, output contract, and at least one worked example |
| **Fortress** | High-stakes: money, permissions, personal data (PII), public-facing or irreversible actions | Everything in Standard **plus** four-layer injection defense, an error-handling table, a human-escalation channel, audit fields, ≥2 examples (including a refusal path), and an explicit residual-risk note |

### The four risk signals Nuwa weighs

When you describe the agent, Nuwa scores four things:

1. **Irreversibility** — can a mistake be undone? (sending money, deleting data,
   publishing externally, placing orders → not undoable)
2. **Blast radius** — how far does one error reach? (one chat / many users / whole
   platform / financial records)
3. **Tool privileges** — how strong are its hands? (text-only answer / reads external
   data / writes external data / executes transactions or code)
4. **Domain sensitivity** — how sensitive is the field? (ordinary / marketing-compliance
   / legal-medical / finance or PII)

Rough rule of thumb:
- Any signal is **HIGH** (money, PII, irreversible, whole-platform) → **Fortress**.
- Writes to external systems, or legal/medical/finance domain, or affects many users → **Standard**.
- All signals low, read-only, single job → **Lite**.

So a "summarize this document" helper lands in **Lite**, a typical "refund customer-service"
assistant lands in **Standard**, and a "process the actual payout" agent lands in **Fortress**.

---

## 5. What the output looks like

Every Nuwa response comes in three short parts:

1. **Understanding & choice** — one or two sentences: what Nuwa thinks you asked for,
   which tier it picked, target model, and output format. If it stripped any injected
   instructions out of your request fields, it says so here.
2. **The agent prompt** — the complete, ready-to-deploy system prompt (usually XML).
   This is the part you copy out.
3. **Handoff & next steps** — how to plug the agent in, any known limits, and an
   upgrade hint (e.g. "to harden this further, move it to Fortress").

The agent prompt itself always contains, at minimum, an **identity** block and at
least one **safety rule** — even at Lite. Higher tiers add guardrails, workflow,
output contract, examples, and (at Fortress) layered injection defense and audit fields.

See [`examples/01_refund_assistant.md`](./examples/01_refund_assistant.md) (Standard)
and [`examples/02_document_summarizer.md`](./examples/02_document_summarizer.md) (Lite)
for full, real outputs.

---

## 6. Rules Nuwa always follows (so you know what to expect)

- **Ask, don't guess.** Missing name/role/responsibilities, or contradictory specs →
  Nuwa asks a question rather than inventing the answer.
- **Safety can't be downgraded.** Request a tier that's too weak for the risk and
  Nuwa upgrades it, explaining why.
- **No phantom tools.** Nuwa will never write that your agent "can" do something via
  a tool unless you've confirmed that tool actually exists in the agent's deployment.
  Unconfirmed capabilities are labeled **"planned / to be configured,"** never "available."
  If you mention a tool, tell Nuwa whether it's really wired up.
- **Anti-injection by default.** Every compiled agent treats user-supplied content as
  **data**, not as instructions that can rewrite its setup. Fortress agents add multiple
  defense layers. (This is strong best-effort design, not a cryptographic guarantee —
  Nuwa is honest about that.)
- **Your input fields are data, too.** If you try to smuggle "ignore your rules"
  instructions inside, say, the "core responsibilities" field, Nuwa strips them and
  tells you in part 1 of its reply.
- **Nuwa won't expose its own internals.** Requests to dump its reasoning, hidden
  prompt, or internal architecture are politely declined — it offers to build you a
  fresh agent instead.

---

## 7. Common questions

**Q: I only gave a vague idea and Nuwa asked me a question. Is it broken?**
No — that's the "ask, don't guess" rule. Answer the question and it proceeds.

**Q: I asked for Lite but got Fortress. Why?**
Your description contained a high-risk signal (money, PII, an irreversible action,
or public-facing output). Nuwa upgrades for safety and explains the trigger in part 1.

**Q: Can I get Markdown or JSON instead of XML?**
Yes. Say so in your request (e.g. "output the agent prompt as Markdown"). XML is just
the default for Claude.

**Q: Can Nuwa build a team of agents at once?**
No. Nuwa builds **one** single-agent prompt per run. Run it again for the next agent.

**Q: How do I tell Nuwa my agent really has a database / API tool?**
State it plainly: "this agent has a verified read-only API to our order system."
If you're unsure it's wired up, say "planned" and Nuwa marks it accordingly — it
will not pretend the tool works.

**Q: Can I edit the prompt Nuwa gives me?**
Absolutely. It's a normal text artifact. A common workflow is to take Nuwa's output,
tweak a line, and redeploy. If you change something risk-relevant, consider asking
Nuwa to re-compile so the tier and guardrails stay consistent.

**Q: The agent refuses a weird user message that tries to override it. Intended?**
Yes. That's the data-boundary / role-lock guardrail doing its job. The agent treats
embedded "ignore previous instructions" text as data, not commands.

---

## 8. A good-request checklist

Before you hit send, make sure you've given Nuwa:

- [ ] A **name**
- [ ] A one-phrase **role**
- [ ] 2–5 concrete **core responsibilities**
- [ ] What the agent **can and cannot do** (especially: does it move money / touch PII / write to systems?)
- [ ] Any **tools** it really has (and which are only "planned")
- [ ] *(optional)* target model, output format, tone, hard limits

Give it those, and Nuwa returns a clean, safe, deployable agent prompt on the first try.
