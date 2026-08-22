# Persona Auditor

<div align="center">

**Find the bugs real users hit that you'd only discover through costly trial-and-error — or never notice at all — and collapse them into a handful of root causes, so your agent fixes precisely instead of patching blindly.**

从真实用户视角审计 AI 代码，找出你反复试验才能发现、甚至永远发现不了的错误，归因成少数根因，**一次修对**。

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Type: agent skill](https://img.shields.io/badge/type-agent--skill-8A2BE2.svg)](https://github.com/kevinshi3200/persona-auditor)
[![skills.sh](https://skills.sh/b/kevinshi3200/persona-auditor)](https://skills.sh/b/kevinshi3200/persona-auditor)

</div>

---

## TL;DR

### The Problem

AI writes code that **runs fine and is internally consistent** — but the moment a real user walks the journey, it falls apart:

- **Users get stuck in the first few steps**, even though every function looks correct in isolation.
- **You patch one point after another, for many rounds, and never find the root cause** — because each fix only sees a local symptom.
- Worst of all: **some bugs you'd never notice at all.** Cross-project contamination, stale state, wrong sort order — they hide deep in the code and only surface when a *real user* walks the *full path*.

### The Solution

Persona Auditor dispatches **"digital personas"** — subagents that each play a real user type — and has them **exhaustively traverse every user path on paper** (no execution, no clicking: cheaper and more global than real clicking). Then it steps back to a **god's-eye view** and attributes dozens of symptoms to **a few root causes**, each with a precise `file:line` and a **falsifiable way to verify the fix**.

```
audit → report (root cause + location) → you confirm → agent fixes → verify the symptom disappears
```

### Why Persona Auditor?

| Strength | What it means for you |
|---|---|
| 🎭 **Real-user perspective** | Simulates *people*, not machine operations, not code logic |
| 👁️ **Finds what you'd never notice** | Hunts "self-consistent but experience-wrong" gaps, not just runtime bugs |
| 🎯 **God's-eye attribution** | Counterfactual reasoning collapses dozens of symptoms into a few root causes — each falsifiable |
| 🧮 **Exhaustive, not sampled** | Equivalence classes + boundary values + orthogonal arrays guarantee no missed combination, with a provable coverage count |
| 🎲 **Deterministic & reproducible** | A bounded set of personas — same input, same output, no chaotic surprises |
| 🔄 **Fix loop, not a report** | Ends in precise fixing + verification, not "audit done, good luck" |
| 🧹 **Release hygiene** *(exclusive)* | Catches AI's signature disease: secrets, PII, and trial-and-error traces written into code |
| 🔍 **Wheel-reinvention audit** *(exclusive)* | Flags hand-rolled modules that a mature open-source library already beats |
| ⚖️ **Adapts to the project** | Lite / Standard / Deep — a landing page doesn't get a heavyweight audit |
| 🔌 **Plugs into your loop** | One-shot, incremental-after-each-change, or a gate before commit/release |

---

## Design Philosophy

1. **Paper traversal is the means, not the end.** "Not executing, not clicking" is how we *exhaust every path* — faster and more global than real clicking. The end goal is **precise fixing** — finding the few root causes so you fix once, not patch forever.
2. **Exhaustiveness is the soul; sampling is negligence.** One happy path proves nothing. The full `persona × function × operation × state × timing` matrix is traversed, with coverage quantified and provable.
3. **Code is the single source of truth.** We reason from the behavior recovered by *reading the code*, not from docs or memory.
4. **Deterministic, not chaotic.** Code and state machines are deterministic, so the simulation is deterministic too — a bounded set of personas, reproducible results.
5. **Falsifiable or it isn't a finding.** Every root cause must carry a verification path ("if I fix X, symptom Y disappears"). No verification path = downgraded to "impression", never reported as a conclusion.

---

## What it detects

Seven dimensions, dozens of checks — one table to show the coverage:

| Dimension | What it detects |
|---|---|
| 🧠 **Logic & state** | logic contradictions, dead branches, deadlock, infinite loops, state pollution, stale state, missing preconditions, variables accidentally overwritten, checks skipped, race conditions |
| 👤 **User experience** | the "self-consistent but experience-wrong" gap, cognitive overload, feature fragmentation, missing feedback, "says vs does" mismatch, discoverability, recoverability, trust building, mental-model mismatch, abandonment points |
| 🔒 **Security** | SQL injection, XSS, SSRF, IDOR, privilege escalation, command injection, path traversal, arbitrary file read, races, prompt injection, jailbreak (34 classes) |
| 🤖 **AI-smell & maintainability** | naming emptiness, comment clichés, over-abstraction, swallowed exceptions, TODO graveyard, over-engineering, reinvented wheels, outdated tech, worse-than-mature-OSS |
| 📋 **Compliance & release** | software-copyright compliance, store-review gating, missing EULA/copyright notices, secret leakage, real PII leakage, internal info / trial-and-error traces leaked, defensive-content pollution (corrections / warnings / self-reminders in public artifacts) |
| ⚡ **Concurrency & stress** | multi-task concurrency, races, timeouts, interruption, duplicate submission, rapid switching, multi-subagent conflicts |
| 🔗 **Cross-project** | cross-project memory contamination, precise navigation, dynamic memory pool, cross-domain routing |

> Checks are **triggered on demand, not dumped on you.** The audit first asks "who is it for, where does it ship" and skips what doesn't apply — no store review for something not shipping, no mobile App Store review for a desktop app.

---

## How it works

1. **Intake** — the agent infers the project type, users, and release scenario from code, then asks you only what code *can't* tell it (design intent, real user pain points, release decision).
2. **Exhaustive traversal** — equivalence classes + boundary values + orthogonal arrays cover the full path matrix without combinatorial explosion.
3. **Persona simulation** — digital-persona subagents (≤10) each walk the full code chain, predicting what their user would do, see, and misunderstand.
4. **God's-eye attribution** — counterfactual reasoning separates root causes from symptoms and ranks them by impact.

---

## Quick Start

```bash
# skills ecosystem (Claude Code / Codex / Cursor / other agents)
npx skills add kevinshi3200/persona-auditor

# or manual: copy SKILL.md into your agent's skills directory
# e.g. ~/.agents/skills/persona-auditor/SKILL.md
```

Then trigger it in natural language:

> "Audit this project with Persona Auditor."
> "审计这个项目" / "找 bug" / "推演" / "验收" / "出包前检查"

**What you get:** a report whose conclusion is **a few root causes + impact-ranked fix directions** — each with `file:line` and a falsifiable verification method — not dozens of symptoms laid flat.

---

## How it compares

| | Code review | E2E / click testing | Chaotic multi-agent | **Persona Auditor** |
|---|---|---|---|---|
| Perspective | code logic | machine operations | emergent agents | **real users** |
| Finds | code quality | runtime bugs | unpredictable | **"self-consistent but experience-wrong" gaps + bugs you'd never notice** |
| Root cause | single point | single point | unclear | **god's-eye counterfactual attribution (a few root causes)** |
| Coverage | sampled | one path at a time | chaotic | **exhaustive, provable coverage** |
| Cost | cheap, shallow | expensive | unpredictable | **cheap + global + reproducible** |

---

## Limitations

- **Read-only during the audit** — it never modifies code (to avoid contaminating the scene). Fixing happens in the loop, *after you confirm*.
- **Cannot** read binaries / reverse-engineer, replace real UI visual inspection, or replace real execution regression.
- Paper traversal surfaces *specification-layer* contradictions; if actual runtime behavior differs from what the code implies, that gap is flagged and fed back into the model.

---

## FAQ

**Will it drown me in findings?**
No. Checks are triggered on demand based on your release scenario, and the final report converges to **a few root causes ranked by impact**, not a wall of symptoms.

**Is it deterministic, or another chaotic multi-agent simulation?**
Deterministic. Code and state machines are deterministic, so it uses a bounded set of personas (≤10, one per persona combo) — reproducible and predictable.

**Does it change my code?**
Not during the audit. After you confirm the report, the same agent fixes precisely per the report and verifies the symptom disappears.

**Does it do a heavy audit on a tiny project?**
No. It picks a depth tier — 🟢 Lite for a landing page / personal tool, 🟡 Standard for most projects, 🔴 Deep for store shipping / sensitive data / agent systems — and only runs what the project needs.

---

## Author & Feedback

- **Author:** [kevinshi3200](https://github.com/kevinshi3200)
- **Email:** [915538592@qq.com](mailto:915538592@qq.com)
- **Issues / feedback / feature requests:** [open an issue](https://github.com/kevinshi3200/persona-auditor/issues)

## License

[MIT](./LICENSE)
