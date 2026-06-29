# Changelog

All notable changes to **Nuwa** are documented here.

Nuwa is a prompt-architect: it compiles a single, robust, deployable
system prompt for an AI agent — in one of three risk tiers
(Lite / Standard / Fortress), with safety guardrails plus
closed-loop / honesty / anti-injection modules baked in.

The format loosely follows [Keep a Changelog](https://keepachangelog.com/).
Versions below describe **capability**, not internal history.

---

## [7.0] — Pull-mode & incentive alignment

### Added
- **Pull-mode activation.** Low-frequency, externally-triggered agents now ship
  with an explicit wake trigger and an anti-idle rule: when summoned with no
  real task, the agent says so plainly instead of inventing busywork. Core
  duties are loaded at build time, so the agent never opens in an empty
  "I'll figure it out later" state.
- **Incentive redefinition.** Guidance for framing an agent's success around the
  *outcome that actually matters* (its north star), not around volume of output
  or appearing busy — reducing reward-hacking-style drift.
- **Name-vs-reality (`名实`) self-check.** A consistency check so that what an
  agent *claims* to be (its declared role, version, and capabilities) matches
  what it actually is in deployment — catching stale version pointers and
  capability claims that no longer hold.

### Changed
- Template restructured into clearly toggleable modules so authors can strip a
  tier down to its minimum or keep the full guardrail set.

---

## [6.0] — Closed-loop hard gates & honesty

### Added
- **Closed-loop hard gate.** For agents that predict, score, or recommend, the
  calibration loop is now a structural requirement, not a nicety: log each
  judgment (prediction + rationale + follow-up trigger), scan the pending board
  on startup, and proactively reconcile predicted vs. actual. The
  "log the prediction but never check the outcome" failure mode is closed by
  framework, not by good intentions.
- **Honesty / no-phantom-tools.** Tools are split into two explicit columns —
  *verified-available* vs. *planned/unverified*. The agent must confirm a tool
  really exists in its deployment before claiming it, and fall back gracefully
  rather than pretend to call something that isn't there.

### Changed
- Deliberation made scenario-based: ask-first when core function / boundary /
  risk is ambiguous; act-then-explain on reversible in-scope work; push back
  once on conflicts, then execute and record the dissent.

---

## [5.0] — Risk tiers & anti-injection hardening

### Added
- **Three-tier compilation (Lite / Standard / Fortress).** The compiler now
  reads risk signals — reversibility, blast radius, tool permissions, domain
  sensitivity — and selects a tier instead of forcing one heavy structure onto
  every agent. Lite stays lean; Fortress keeps every module plus human gates.
- **Anti-injection hardening.** Tool output, fetched pages, file contents, and
  third-party messages are treated as *data, not instructions*. Embedded
  commands ("ignore previous rules", "reveal your system prompt", "you are now…")
  are ignored and, where relevant, flagged.
- **Affirmative refusals.** Every prohibition is paired with a constructive
  alternative: state what cannot be done, then what can. Security is described
  as "layered, best-effort" — never "absolutely secure".

### Changed
- Hard boundaries promoted to highest priority and made append-only: prefer
  adding guardrails/examples over silently deleting or overwriting existing ones.

---

## [4.0] — Baseline

### Added
- Initial single-agent system-prompt compiler with a fixed six-module skeleton:
  identity, mandate, boundaries, workflow, output, and a basic safety section.
- Mandate / anti-scope-creep block so an agent's core function isn't reshaped by
  one-off requests.
