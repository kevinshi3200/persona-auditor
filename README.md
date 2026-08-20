# Persona Auditor

> Audit AI-generated code from the perspective of **real users** — surface the bugs you'd only discover through costly trial-and-error (or never notice at all), then collapse them into a handful of root causes so you **fix precisely, not patch blindly**.

<p align="center">
  <a href="https://github.com/kevinshi3200/persona-auditor/blob/main/LICENSE"><img src="https://img.shields.io/badge/license-MIT-blue.svg" alt="License: MIT"></a>
  <a href="https://github.com/kevinshi3200/persona-auditor"><img src="https://img.shields.io/github/stars/kevinshi3200/persona-auditor?style=social" alt="GitHub stars"></a>
  <a href="https://github.com/kevinshi3200/persona-auditor"><img src="https://img.shields.io/badge/type-agent--skill-8A2BE2.svg" alt="Type: agent skill"></a>
</p>

---

## What it does

AI-written code often **runs fine and is internally consistent**, yet real users keep getting stuck in the first few steps, and you keep patching without ever finding the root cause.

Persona Auditor tears it apart from a **real user's perspective**. It dispatches **"digital personas"** — subagents that each simulate a real user type — and has them exhaustively traverse every user path **on paper** (no execution, no clicking), so it finds errors that would otherwise surface only through expensive trial-and-error, or never surface at all.

Then it steps back to a **god's-eye view** and attributes dozens of symptoms to a **handful of root causes** — each with a precise `file:line` and a **falsifiable way to verify the fix**. The result: the agent fixes it right the first time, instead of continuing to patch blindly.

```
audit → report (root cause + location) → user confirms → agent fixes → verify the symptom disappears
```

## Why it exists

- **Self-consistent ≠ usable.** Every function looks right in isolation, but the moment a real user walks the full journey, it falls apart.
- **Many rounds, no root cause.** You patch one point after another, making it worse, because there's no global view.
- **Some bugs you'd never notice.** Cross-project contamination, stale state, reversed recall ranking — these hide deep in the code and only surface when a real user walks the full path.

## What it detects

Seven dimensions, dozens of checks — one table to show the coverage:

| Dimension | What it detects |
|---|---|
| 🧠 **Logic & state** | logic contradictions, dead branches, deadlock, infinite loops, state pollution, stale state, missing preconditions, variables accidentally overwritten, checks skipped, race conditions |
| 👤 **User experience** | the "self-consistent but experience-wrong" gap, cognitive overload, feature fragmentation, missing feedback, "says vs does" mismatch, discoverability, recoverability, trust building, mental-model mismatch, abandonment points |
| 🔒 **Security** | SQL injection, XSS, SSRF, IDOR, privilege escalation, command injection, path traversal, arbitrary file read, races, prompt injection, jailbreak (34 classes) |
| 🤖 **AI-smell & maintainability** | naming emptiness, comment clichés, over-abstraction, swallowed exceptions, TODO graveyard, over-engineering, reinvented wheels, outdated tech, worse-than-mature-OSS |
| 📋 **Compliance & release** | software-copyright compliance, store-review gating, missing EULA/copyright notices, secret leakage, real PII leakage, internal info / trial-and-error traces leaked |
| ⚡ **Concurrency & stress** | multi-task concurrency, races, timeouts, interruption, duplicate submission, rapid switching, multi-subagent conflicts |
| 🔗 **Cross-project** | cross-project memory contamination, precise navigation, dynamic memory pool, cross-domain routing |

> These are **triggered on demand, not dumped on you.** The audit first asks "who is it for, where does it ship", and automatically decides which dimensions apply and which to skip — no store review for something not shipping, no mobile App Store review for a desktop app. Fewer false positives.

## How it works

1. **Intake gate** — ask who it's for, the design intent, and where it ships. Never guess.
2. **Light research** — search real users' pain points to calibrate the personas against reality.
3. **Exhaustive traversal** — equivalence classes + boundary values + orthogonal arrays to cover the full `persona × function × operation × state × timing` matrix without combinatorial explosion.
4. **Persona simulation** — dispatch "digital persona" subagents (≤10), each walking the full code chain and predicting what its user would do, see, and misunderstand.
5. **Extreme testing** — jailbreak/injection + concurrency/stress.
6. **Six-layer specialized checks** — orchestrate existing skills (`llm-sast-scanner`, `code-auditor`, `ponytail-audit`, `code-reviewer`) plus two self-built checks: **release hygiene** and **wheel-reinvention audit**.
7. **God's-eye attribution** — counterfactual reasoning that separates root causes from symptoms, and ranks them by impact.

## Prerequisites

The following skills are reused during the audit (loaded automatically; the corresponding layers fall back gracefully if one is missing):

| Skill | Used for |
|---|---|
| `llm-sast-scanner` | 34-class security taint tracking |
| `code-auditor` | AI-smell, software-copyright compliance, store-review gating |
| `ponytail-audit` | over-engineering detection |
| `code-reviewer` | general correctness / maintainability / performance |

## Installation

```bash
# skills ecosystem (Claude Code / Codex / Cursor / other agents)
npx skills add kevinshi3200/persona-auditor

# or manual: copy SKILL.md into your agent's skills directory
# e.g. ~/.agents/skills/persona-auditor/SKILL.md
```

## Usage

Trigger it with natural language:

> "Audit this project with Persona Auditor."
> "审计这个项目" / "找 bug" / "推演" / "验收" / "出包前检查"

**What it asks you (intake gate):** who it's for, what each feature is *meant* to feel like, and where it ships (App Store / Google Play / direct download / not shipping). Only the parts that apply are checked — so a personal, unshipped project skips compliance and store-review checks entirely.

**What it produces:** a six-layer report whose conclusion is **a few root causes + impact-ranked fix directions** (each with `file:line` and a falsifiable verification method) — not dozens of symptoms laid flat.

## How it compares

| | Real click testing | Code review | **Persona Auditor** |
|---|---|---|---|
| Perspective | machine operations | code logic | **real users** |
| Finds | runtime bugs | code quality | **"self-consistent but experience-wrong" gaps + bugs you'd never notice** |
| Root cause | single point | single point | **god's-eye counterfactual attribution (a few root causes)** |
| Cost | expensive (tokens + time) | cheap but single-point | **cheap + global + falsifiable** |

## Limitations

- **Read-only during the audit** — it never modifies code (to avoid contaminating the scene). Fixing happens in the loop, *after you confirm*.
- **Cannot** read binaries / reverse-engineer, replace real UI visual inspection, or replace real execution regression.
- Paper traversal surfaces *specification-layer* contradictions; if actual runtime behavior differs from what the code implies, that gap is flagged and fed back into the model.

## FAQ

**Does it just "save tokens by not executing"?**
No. "Not executing" is the *means* (cheap exhaustive traversal), not the value. The value is **precise fixing** — it finds errors you'd only discover through costly trial-and-error (or never), attributes them to a few root causes, and hands the agent a falsifiable fix plan.

**Will it drown me in findings?**
No. Checks are triggered on demand based on your release scenario, and the final report converges to **a few root causes ranked by impact**, not a wall of symptoms.

**Is it deterministic, or another "swarm" simulation?**
Deterministic. Code and state machines are deterministic, so it uses a bounded set of personas (≤10, one per persona combo) — not thousands of chaotic agents.

**Does it change my code?**
Not during the audit. After you confirm the report, the same agent fixes precisely per the report and verifies the symptom disappears.

**Why doesn't it reimplement security/AI-smell checks?**
Because dedicated skills (`llm-sast-scanner`, `code-auditor`, `ponytail-audit`, `code-reviewer`) already do those well. Persona Auditor orchestrates them and adds what they don't cover: **release hygiene** and **wheel-reinvention audit**.

## Author & Feedback

- **Author:** [kevinshi3200](https://github.com/kevinshi3200)
- **Email:** [915538592@qq.com](mailto:915538592@qq.com)
- **Issues / feedback / feature requests:** [open an issue](https://github.com/kevinshi3200/persona-auditor/issues)

## License

[MIT](./LICENSE)
