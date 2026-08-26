---
name: persona-auditor
description: >
  Persona Auditor audits AI-generated code from the perspective of real users,
  surfacing the errors you would only discover through costly trial-and-error — or never notice at all —
  and collapsing them into a handful of root causes via god's-eye attribution, so you fix precisely instead of patching blindly.
  It dispatches "digital personas" that simulate real users and exhaustively traverse every user path on paper —
  cheaper and more global than real clicking, without executing any code.
  Built for AI-written code that runs fine and is internally consistent, yet keeps users stuck in the first few steps
  and defies root-cause diagnosis across many rounds of fixes.
trigger:
  - audit
  - test
  - find bug
  - walkthrough
  - simulation
  - acceptance
  - user journey
  - logical reasoning
  - paper traversal
  - pre-release check
  - app store review
  - pre-launch check
  - 审计
  - 测试
  - 找 bug
  - 推演
  - 验收
  - 用户旅程
  - 逻辑推演
  - 纸面推演
  - 出包前检查
  - 上架前检查
  - 发布前检查
  - logic contradiction
  - infinite loop
  - deadlock
  - race condition
  - state pollution
  - stale state
  - concurrency
  - boundary value
  - equivalence class
  - orthogonal array
  - SQL injection
  - XSS
  - SSRF
  - IDOR
  - privilege escalation
  - prompt injection
  - jailbreak
  - security audit
  - AI smell
  - over-engineering
  - reinvent the wheel
  - tech-stack unification
  - adapter convergence
  - parallel adapters
  - dead adapter
  - write-only config
  - one concern two adapters
  - multiple adapters
  - 技术栈统一
  - software copyright compliance
  - store review
  - secret leakage
  - release hygiene
  - defensive content pollution
  - guardrail leakage
  - internal metadata leak
  - self-reminder leakage
  - cross-project contamination
  - 逻辑矛盾
  - 死循环
  - 死锁
  - 竞态
  - 状态污染
  - 状态残留
  - 并发
  - 边界值
  - 等价类
  - 正交数组
  - SQL 注入
  - XSS
  - SSRF
  - IDOR
  - 越权
  - 提示词注入
  - 越狱
  - 安全审计
  - AI 味
  - 过度工程
  - 造轮子
  - 软著合规
  - 商店审核
  - 密钥泄露
  - 发布卫生
  - 防御性内容污染
  - 纠正泄漏
  - 内部备注泄漏
  - 自我提醒泄漏
  - 跨项目串台
---

# Persona Auditor（数字人审计员）

## 1. Positioning & Iron Rules

**Positioning**: a **comprehensive audit-orchestration skill** whose goal is **precise fixing** — find the errors the user would only discover through costly trial-and-error (or never notice at all), collapse them into a handful of root causes, and let the agent fix it right the first time. It is built from four parts:

1. **Main line (the differentiator)**: user personas → project structure map → state-machine exhaustion → two-layer persona traversal → experience-layer three-way alignment → reference group → god's-eye attribution. It hunts the "contradiction between underlying logic and user experience" — built for "stuck on one point across many rounds of fixes, missing the global view".
2. **Orchestrated reuse (don't reinvent)**: for security / AI-smell / over-engineering / store review / general correctness, load and run existing skills and merge their results into one unified report.
3. **Self-built checks (not covered by any existing skill)**:
   - **Release hygiene**: development trial-and-error, conversations, summary conclusions, real personal information, secrets, and internal decisions written into code / config / comments / docs.
   - **Defensive-content pollution**: guardrail metadata (corrections, risk warnings, discipline, self-reminders) leaked into public deliverables and user-visible strings.
   - **Wheel-reinvention audit**: "this whole module has a more mature open-source alternative, but you hand-rolled an outdated one".
   - **Adapter-convergence audit (tech-stack unification)**: "the same concern was solved N times, in N parallel ways, and never converged to one seam" — the subagent/borrowed-tech failure mode that leaves one concern with N adapters (or 1 live + 1 dead), a write-only capability registry, or two names/units for one fact.
4. **Adaptive scope**: depth tiers (Lite/Standard/Deep) and loop modes (one-shot/incremental/gate) so the audit weight matches the project, never over- or under-auditing.

**Paper traversal is the means, not the end**: not executing, not clicking is how we *cheaply exhaust every user path* — faster, more global than real clicking, able to see root causes at a glance. **But it is never an excuse to "not fix"**: the audit's closed loop is `report (root cause + precise location) → user confirms → agent fixes precisely → verify via falsifiable check (symptom disappears)`.

**Iron rules** (9, not a wall — each one earns its place):
1. **Do not execute code, do not really click, do not modify code** — mainline work happens on paper.
2. **Code is the single source of truth**: reason from the real behavior recovered by reading code, not from design docs or memory.
3. **Do not fall into the "code self-consistency" trap**: internally consistent code ≠ a good product.
4. **Exhaustiveness is the soul; sampling is negligence**: steps 1.5/2/3 must fully traverse the `structure × persona × function × operation × state × timing` matrix; many walker subagents each reason independently; **only after traversing all paths may you do root-cause analysis**. Forbidden to sample one happy path and conclude.
5. **Reuse first, don't reinvent**: for security / AI-smell / maintainability / review, load `llm-sast-scanner` / `code-auditor` / `ponytail-audit` / `code-reviewer`; do not rewrite their rules.
6. **The self-built checks (release hygiene + defensive pollution + wheel reinvention + adapter convergence) are this skill's own — always run them, never skip.**
7. **Read-the-code verification spans multiple steps, not just once at step 1**: walker traversal (step 3), extreme testing (step 4), three-way alignment (step 10), and attribution (step 11) must each **re-read the code to verify**; forbidden to conclude from step-1 memory. Every `file:line` must be "just read / just grepped in this round", not "from memory" — memory goes stale and hallucinates.
8. **No false positives, no over-auditing**: before reporting anything, confirm "does this scenario apply?" — no store review for something not shipping, no mobile App Store review for a desktop app, no treating test fixtures / vendored code / build artifacts / .env templates / doc TODO placeholders as production bugs. And match the audit weight to the scenario — never run the heavy pipeline on a landing page. **A wrong report hurts trust more than a missed one.**
9. **Honest + falsifiable**: if you can't find it, say what's missing — never fabricate. Every root-cause conclusion must carry a falsifiable verification method ("if I fix X, symptom Y disappears; if Y persists, X was wrong"); an unfalsifiable statement is an impression, tagged `【impression, no verification path】`, never a finding.

---

## 2. Methodology (Intake Gate + Steps)

### Intake Gate (mandatory, never skip, never guess)

**Core principle: the agent answers first, the human only fills what AI can't know.** This skill is for the agent itself — it can read code, README, and context. So most questions are **answered by the agent first, inferred from code** (with evidence + confidence), not dumped on the human. Only what "isn't in code, only in the founder's head" gets asked of the human.

**Round 1 — agent self-answers (infer before asking).** Read code + README + context; answer each item with `inferred answer + evidence + confidence (high / medium / low)`:

1. **Project type** — web app / CLI / library / desktop app / agent system / game / other (from README / package.json / structure). **This decides what "structure unit" means in Step 1.5.**
2. **Target users** — how many types, what habits (reverse-infer from README / copy / features).
3. **Release scenario** — ships where? personal vs commercial? software-copyright? (infer from package.json / README / deploy config; decides whether compliance / store-review checks apply).
4. **Type-specific follow-ups** — web/desktop: which journeys are core, which step loses users, any target platform; CLI/library: target devs, integration scenarios, API constraints; agent system: interaction mode (chat/canvas/voice), automation boundary (what must never auto-run); game: target players, core fun, payment/store plan.
5. **Depth tier** — Lite / Standard / Deep, with a one-line reason (see "Audit Depth Tiers").
6. **Loop mode** — one-shot / incremental / gate (see "Embedding in an Existing Loop").

**Round 2 — human fills the gaps (only what code can't answer).** From Round 1, take the items with **low confidence** or that **change the audit conclusion**, and ask the human as **multiple-choice** — each question shows the agent's inferred answer as the **recommended option**, a few alternatives, and a **"skip — use your inference"** option.

**Always ask the human (code never contains these):**
1. **Design intent** — for each core feature, what experience did you *want* the user to get? (code shows what it does, not what it means to achieve). **This is also the ruler for gate vs break in Step 3a.**
2. **Real user thoughts** — the actual habits / tolerance / pain points of your target users (not in code).
3. **Release final say** — does it actually ship / register copyright? (partly inferable, but you decide).
4. **Is the requirement itself right?** — only when it arises (A/B reference-group choice).

**Fallback rule**: skipped items use the agent's inferred value, tagged `【agent-inferred, not user-stated, needs confirmation】`. If a point arises that "only the user can decide" (A/B reference group, requirement validity, wheel replacement, depth tier, loop mode), **stop and ask — never decide for them**.

### Audit Depth Tiers (adapt the weight to the project)

**Not every project needs a heavy audit.** The agent infers a tier from the intake answers, recommends it with a reason, and lets the user override.

How to decide: ① project size ② purpose (self-use/internal/commercial/shipping) ③ release scenario ④ risk surface (user data? concurrency? auth/payment? AI mainline?) ⑤ user intent ("quick look" vs "full acceptance").

| Tier | For | Runs | Skips |
|---|---|---|---|
| 🟢 **Lite** | landing page, one-off script, personal tool, self-use | quick classify; 1–2 personas on the 1–3 core paths; experience alignment (core only); quick release-hygiene sweep; simplified attribution | six-layer checks, wheel-reinvention, extreme testing, concurrency stress, orthogonal-array exhaustion, light research |
| 🟡 **Standard (default)** | medium project, internal tool, acceptance | full steps; six-layer checks trimmed by release scenario | compliance layer if not shipping / no copyright |
| 🔴 **Deep** | store shipping, copyright, commercial release, high concurrency, sensitive data, agent system, "full acceptance" | everything: all steps + all six layers + extreme testing + concurrency stress | nothing |

Rules: **recommend, don't decide** (state tier + reason, let user override); **never silently escalate** (a big codebase alone is not a reason for Deep); **record the tier honestly** in the report header.

### Embedding in an Existing Loop

| Mode | When | Scope | Output |
|---|---|---|---|
| **One-shot** | standalone full audit | everything | full report |
| **Incremental** | after each change in the loop | `git diff` + paths the diff touches (inferred from the state machine) | short: new/recurring blockers + root cause |
| **Gate** | before commit / merge / release | high-risk layers only (high-severity security + release hygiene + core paths) | PASS / BLOCK + blocking list |

Execution principles: **baseline once, then increment** (first run builds personas + state machine + root causes; later runs are Incremental and reuse it — never re-run full unless asked or structure changed); **incremental scope** = `git diff` + reverse-infer affected paths; **gate pass criteria stated up front**; **match output to the loop** (full report is useless mid-loop, bare PASS/FAIL useless pre-release).

### Light Research (after intake gate, before step 0; mandatory for Standard/Deep, skippable for Lite)

**Personas must not be invented off the top of your head.** A round of light research (capped at 3–5 searches) to get three things: ① target-user commonalities ② real pain points ③ behavior patterns. **Disciplines**: every conclusion carries a source URL; when research conflicts with the user-stated intake answers, **user-stated wins** (the founder knows their product better than external research), but list the conflict explicitly.

### Step 0: Set the ruler — user-persona layering (targeted)

Based on **three-way synthesis**: intake gate (human-stated) + agent self-answer (code-inferred) + light research (external data), produce:

- **User-persona layering table**: ≥3 user types; for each: usage habits / expectations / tolerance / features they will and won't use / pain points / behavior patterns. Tag each item's source: 【user-stated】/【agent-inferred】/【light research · URL】.
- **Design-intent table**: for each feature, "what experience the designer wanted the user to get", source-tagged. **This table doubles as the gate list for Step 3a's gate-vs-break distinction.**

### Step 1: Recover the truth — read code and model the state machine

- Recover the **real state machine**: state variables (with initial values), transition conditions, branches, boundaries, race points.
- Recover the **real behavior of each feature** (what the code actually does, not what the docs claim).
- Annotate: deviations between actual code behavior and design intent.

### Step 1.5: Project structure map (the "brain" — see the whole before dispatching)

**Before exhaustively walking paths, list what this project actually contains — so the walkers are assigned, not left to guess.** Read code (not docs alone) and produce:

1. **Structure units** — what this project is made of, in *its own* organizing terms (never preseed a term):
   - web app → pages / routes / feature modules
   - CLI → commands / subcommands
   - game → play systems / mechanics
   - agent system → capability scenarios (whatever the project itself calls them)
   - library → modules / API surfaces
2. **Core journeys per unit** — the typical user task in each unit (1–N journeys each).
3. **Journey inventory** — one table of all units × their journeys. **This is the sole input to Step 2.**

**Coverage check (explicit)**: the inventory must state "N units × M journeys; unit A has k journeys, unit B has j…" — a missed unit is a failure, not an option. If the tier is Lite, skip units honestly and say why, don't silently drop them.

### Step 2: Exhaust paths — persona combos + combination-reuse principle

**Iron rule: exhaustion = fully traversing `persona combos × journey × operation × state × timing` (where "journey" comes from Step 1.5's inventory), but "combos" use equivalence classes + boundary values + orthogonal arrays, not the full Cartesian product. Forbidden to sample one happy path and conclude.**

**The no-miss methodology (classic software-testing trio)**:
1. **Equivalence-class partitioning**: personas by "behavioral similarity"; journeys grouped by "same kind of behavior" (all "create X" is one class); operations into "normal / abnormal / boundary / interruption" four classes, pick representative values per class.
2. **Boundary-value analysis**: test **both sides** of numeric/length/count boundaries (0 and 1, upper and upper+1, empty and 1 char) — bugs cluster at boundaries.
3. **Orthogonal arrays**: guarantee "**every pair of dimensions' combination is covered at least once**" for 3+ factors (persona × journey × operation); for 2 factors, full combination.

**Coverage must be quantified and provable**: the report states `covered N personas × M journey groups × K operation classes = P paths; orthogonal table guarantees every two-dimension combination ≥1; boundary values cover 0/1/upper/upper+1`.

**Persona = persona × scenario combination** (not single persona, not full Cartesian product):
1. List **single-scenario personas** (each persona type × each scenario — use the project's own scenario names).
2. List **combined-scenario personas** (real users often "do both A and B").
3. **Combination-reuse principle** (avoids explosion): if "A alone" and "B alone" are each tested fine, the "A+B" persona only tests the **cross-scenario delta**:
   ① **precise switching**: is moving between A↔B precise, reversible, free of leftover wrong state;
   ② **cross-scenario recall**: does the memory/state layer know the relation between A and B, and recall correctly without contamination or loss;
   ③ **is the cross-scenario mechanism working** at all.
   Only fully test a combination when a single scenario wasn't tested or the combo introduces new state.

**Persona count is justified, not arbitrary**: total = needed combos, **capped ≤10**. Over the cap, prioritize "single-scenario first, then high-value combos", and state what was dropped and why.

### Step 3: Two-layer persona traversal — mechanical walker (objective) + intent observer (subjective)

**Key distinction**: deterministic traversal — code and state machines are deterministic; not MiroFish-style chaotic swarm. Split into two layers with **different jobs and different output status**:

- **Layer A — mechanical walker (objective)**: walks every path, records only facts. Output is **falsifiable**.
- **Layer B — intent observer (subjective, ONE persona)**: watches Layer A's facts, produces **reference opinions**. Output tagged `【主观参考, 非可证伪发现】` — a hypothesis generator, not a finding.

#### 3a. Layer A — mechanical walkers (record facts, never judge)

1. **Dispatch walkers**: one per persona combo from step 2. Prompt synthesized from ① light research ② intake gate ③ code reverse-inference.

2. **Each walker verifies along the full code chain and records ONLY four facts per step**:
   - `reached`: which step it reached (read code at each link: `user action → entry function → state transition → tool → persistence → output → new state`);
   - `broke`: did it break here — an **objective, code-verifiable fact**: no way to continue (no available action / state deadlocked / thrown error). Confirm by reading code, tag `file:line`.
   - `gate_or_break`: **is this stop a gate or a break?** A *gate* = an intended human-in-the-loop pause (from Step 0's design-intent table — e.g. confirm/approve/choose points the user deliberately designed). A *break* = an unintended stop (no-key early return, infinite hang with no timeout, thrown error). **Gates are features, not findings; only breaks are defects.** When unsure, tag `gate?` and let attribution decide with the user.
   - `steps`: how many steps / rounds before the stop (operation complexity, NOT wall-clock seconds — paper traversal can't measure real time).

3. **Forbidden for walkers**: do NOT judge "is this a bug / bad UX / would the user be confused" — Layer B / attribution's job. A walker that judges has overstepped; downgrade that to a Layer B hypothesis.

4. **Make factual disagreements explicit**: if two walkers record differently on the same path (state/persona differs), record both verbatim — a data point, not a dispute to smooth over.

#### 3b. Layer B — intent observer (ONE persona; produces hypotheses, never findings)

**Positioning**: not "a simulated user", but a **human-intent observer** — like a senior UX researcher: understands human behavior and product intuition, reads Layer A's *factual* breakpoints, produces *reference opinions*.

**Input**: Layer A's break list (`file:line` + gate_or_break + step counts).

**Output — every item carries three parts + a tag**:
```
【主观参考】关注点: <which break / path segment, anchored to a Layer A fact>
理由: <why a human would most likely care here — behavior / habit intuition>
建议方向: <where attribution should read the code to verify — file:line>
```

**Three disciplines**:
1. Every item MUST be tagged `【主观参考, 非可证伪发现】` — iron rule 9: an opinion is not a finding.
2. It may only say "this looks suspicious, suggest reading code X" — never "this is a bug".
3. Its opinion MUST anchor to a Layer A fact ("based on breakpoint #N…") — never free-floating "I think users would be confused".

**What the observer adds that walkers can't**: prioritization intuition — among many factual breaks, which ones a novice trips on vs an expert ignores, which everyone curses. It reads the *facts* and adds the *human weighting* Layer A deliberately does not do.

#### Aggregation (after both layers)

- Layer A: aggregate breaks — single-user-specific (boundary) vs hit-by-all (systemic); **gates are recorded but not treated as defects**.
- Layer B: attach hypotheses to the systemic breaks.
- **Hand both to Step 11 (god's-eye attribution)**: Layer A facts = hard evidence; Layer B opinions = investigation leads. Attribution reads code to verify each lead before it becomes a finding. An opinion with no code confirmation stays `【主观参考】`, never promoted.

### Step 4: Extreme testing — jailbreak/injection + stress/concurrency

- **Jailbreak / injection**: prompt injection, role hijacking, system impersonation, jailbreak templates, invisible Unicode. Against step 1's "trusted input boundary", find penetration points and correct interception points.
- **Multi-task concurrency stress**: N tasks simultaneously, races, timeouts, interruption, duplicate submission, rapid switching. Traverse who overwrites whom, whether locks fail, whether queues are serialized.

### Step 5: Specialized-check orchestration — reuse existing skills (don't reinvent)

| Dimension | Reused skill | What it checks |
|---|---|---|
| Security | `llm-sast-scanner` | 34 vulnerability classes, Source→Sink taint tracking |
| AI-smell / maintainability | `code-auditor` (AI-smell) + `ponytail-audit` | naming emptiness / comment clichés / swallowed exceptions / TODO graveyard; over-engineering delete/stdlib/native/yagni/shrink |
| Store review | `code-auditor` (copyright + store-gating) | **Conditional**: only when the release scenario says ship/register (already decided in Intake Gate); otherwise skip this row. Know the platform first — an Electron desktop app does not go through App Store/Google Play mobile review |
| General correctness | `code-reviewer` | correctness / security / maintainability / performance 🔴🟡💭 — **must explicitly cover: dead code / speculative abstraction (zero-consumption modules, dead switches, retired-but-kept files), duplicate definitions / same-name conflicts (one symbol defined multiple incompatible ways), god files / over-centralized modules (one file carrying too many responsibilities)** |

Duplicates go to the more specialized one; dedupe then merge. **Before reporting each layer, pass iron rule 8 (false-positive check).**

### Step 6: Release-hygiene check (self-built, mandatory)

**AI-assisted-development-specific — things that should not be publicly released got written into code/config/comments/docs.** Scan item by item:

- **Real personal information**: name / phone / email / address / school / grades / ID number, hardcoded into code, config, fixtures, comments.
- **Secrets and credentials**: API key / secret / token / private key / personal workspace endpoint, written outside .env.
- **Development trial-and-error traces**: comments like "incident observed", "user said XX", "audit found XX", "walkthrough gap N", "temporary / do this for now / to delete".
- **Internal conversations and conclusions**: development-time dialogue, trial-and-error conclusions, internal decisions written into prompts/comments/docs.
- **Internal domain information**: internal project names, private domains, internal architecture decisions, unpublished account systems.

For each hit, tag `【release hygiene】+ file:line + content category + leak level`, and list separately in the report.

### Step 7: Defensive-content pollution (self-built, mandatory)

**The disease**: AI wrote its *internal* guardrail metadata — corrections, risk warnings, discipline clauses, self-reminders, "as an AI" disclaimers — straight into an artifact meant to be public or a string meant for the end user. The artifact stops being universal: a marketing copy with "⚠️ needs verification" can't be published; open-source code with "// untested, don't trust this" can't be shipped; a user-facing message with "产物自己过一眼" leaks a producer-only concern.

**How it differs from release hygiene**: release hygiene leaks *past* dev traces (secrets, PII, trial-and-error); this leaks the *current* artifact's own defense notes. Same root cause, different symptom — a public deliverable and its internal guardrail metadata were not physically separated.

**The three-question test** (apply to every suspicious sentence / comment):
1. **Meta or content?** Is this sentence *about* the artifact (its correctness / trust / status), or *is* it the artifact (the actual copy / code / doc)?
2. **Who is it addressed to?** To the *producer* ("note to self", "for internal review") — or to the *end user*?
3. **Deletion test (falsifiable)**: delete it — is the artifact still complete and *more* universal? If yes, it was internal metadata and must be reported.

**Signals to scan for**:
- Explicit guard markers: `⚠️` / `【备注】` / `【纪律】` / risk warning / disclaimer / "AI-generated, please verify"
- Self-reminder speech: "as an AI I remind you", "please verify", "not tested", "don't ship", "for reference only", "does not constitute advice"
- Code self-notes: `TODO` / `FIXME` / `HACK` / `// note to self` / `// this broke before, don't touch`
- Leaked internal correction: "this was wrong before", "temporary", "for now", "internal wording", "internal decision"

**Where to scan — five surfaces (breadth: not just chat and deliverables)**:

Defensive pollution hides in *every* user-visible string. Scan all five:

1. **Conversation output** — `appendAssistant` / `finalText` / `appendBrief` / dialogue messages.
2. **Deliverable files** — docs / code / config meant to be published or shipped (overlaps release hygiene).
3. **Settings / config UI text** — model pickers, tier labels, capability maps, degradation-matrix wording, parameter descriptions. Internal architecture terms shown to the user. Locate: `displayName` / `label` / `title` / `t(...)` in settings components and registry/archive files.
4. **Runtime result display** — status text, progress, result cards, quality-gate verdicts rendered in the UI. Locate: `statusText` / progress / verdict rendering.
5. **Run log / notifications** — notification center, toast, event timeline, log panel. Internal QA/process terms shown to the user as "history". Locate: `pushNotification` / toast / event feed / log lines that reach the UI.

*(The concrete function names above are examples of "how to locate" each surface — the exact names differ per project; use the project's own user-visible-output entry points.)*

**Rule**: a term is internal if it names an architecture component, a process step, or a producer-only concern. The same term is *not* pollution when the audience is an operator configuring the system (power-user settings may legitimately name engines). Apply iron rule 8: "end-user-facing text" vs "operator-facing text" are different audiences — only the former is pollution.

**Severity**: High = makes the artifact unpublishable; Medium = local contamination; Low = deliberate compliance disclaimer (flag, don't assume it's a bug).

**Root cause (god's-eye)**: guardrail metadata was not physically separated from the deliverable. Triggers: ① guardrail tools set to `on_fail="fix"` rewriting in place; ② prompts telling the model to "self-correct" without distinguishing internal vs external; ③ internal discussion leaking into context and copied into output.

**Fix direction (for the report, not the audit)**: two-layer output (draft vs `public_release`), a `product_mode` parameter, defense metadata routed to a side channel (logs / review queue) — never injected into the payload. The audit detects the leak and points at its root cause; it does not redesign the pipeline.

### Step 8: Wheel-reinvention audit (self-built, mandatory)

**Find "this whole module has a more mature open-source alternative, but you hand-rolled an outdated one."**

1. Enumerate the project's feature modules / self-built dependencies (especially "looks hand-written": auth, state management, message queue, ORM, template engine, utility collections, protocol implementations…).
2. For each suspected module, **search first** (web_search / npm view / GitHub search) for mature alternatives, comparing maturity, maintenance, completeness, license.
3. **Code comparison**: self-built vs mature alternative — where does it fall short?
4. When something is "clearly worse", **stop and ask the user whether to replace** — don't decide unilaterally.

### Step 9: Adapter-convergence audit — tech-stack unification (self-built, mandatory)

**The disease this catches is NOT "a module is bad" — it's "the same concern was solved N times, in N parallel ways, and never converged to one seam."**

This is the failure mode of **subagent / borrowed-tech-driven development**: each subagent or borrow adds its *own* adapter (LLM client, storage layer, renderer, tool dispatcher) for a concern that already had one. Left unmanaged, you get "one concern → N adapters", where N adapter instances each carry lifecycle/params duplicated, or one adapter is dead while a parallel copy is alive. The debt lives in the **interface/architecture layer** (how many ways reach the same capability), not in whether any single module is well-written.

**How it differs from Step 8 (wheel reinvention)**:
- **Wheel reinvention** asks "did you hand-roll a *worse* version of something with a mature OSS alternative?" — the fix is *replace/borrow*.
- **Adapter convergence** asks "do you have *more than one* adapter for the same concern?" — even if each is well-built, *having N is the debt*. The fix is *converge to one seam* (delete redundant adapters, or wire the one that's dead, or make N share a single interface).

**Detection method — a strict, falsifiable, four-pass scan (per concern):**

For each cross-cutting concern (LLM / model, storage / persistence, rendering, tool dispatch, voice I/O, logging, config), run:

1. **Enumerate ALL implementations of that concern** — find every file that claims the concern's job (grep the concern's verbs: "send"/"openai"/"chat", "write"/"store"/"save", "render"/"draw", "call tool"/"dispatch", "transcribe"/"speak"). Do NOT stop at the one the design doc names; grepping is what surfaces the parallel ones.

2. **Find every call-site that reaches the concern** — not just the definitions. A definition is separable from whether it's *reachable*: `grep` the adapter's exported symbol + import path. **A symbol with quotes in a test/script but zero production callers is DEAD (unwired), not "reserved".** Record: `definition (file:line)` + `call-sites (count, file:line)` + `is there a production caller?`.

3. **Count the distinct adapters (the headline finding)**: for the same concern, is it 1 adapter used everywhere (converged — good), or N adapters where N≥2, or 1 live + 1 dead, or N that each hold partial capability? **The cardinality of "ways to reach a concern" IS the finding.**

4. **Verify "single source of truth" for the concern's metadata**: do the adapters read the SAME capability spec (window size, modalities, dialect, limits), or is the same fact stored in **N registries with N field names / N units** (e.g. `contextLimit:1_000_000` vs `windowK:1000`, `vision:true` vs `modalities:['image']`)? N registries = drift risk; a value changed in one and stale in the other is the proof. **This is the "same fact, two copies" check.**

**The six signals that flag a convergence failure** (each falsifiable, pick matching ones):
| Signal | What it means | Prove by |
|---|---|---|
| 🎯 **Dead adapter** (unwired) | a whole adapter module exists, has exported symbols, but zero production callers; only a manual test script imports it | grep the symbol + import path → only hits are the module itself + a non-CI script |
| 🧩 **Parallel adapters** | the same concern solved by N separate implementations (N renderer instances / N storage stores / N tool-dispatch paths) at once | grep count of `new XRenderer` / distinct store/types / distinct dispatch entrypoints |
| 📇 **Write-only capability schema** | a config/registry declares fields (context, limits, dialect, vision, tools) that NO code reads | grep each declared field → zero readers |
| 👻 **Ghost enum value** | a union/type declares a value ('responses') that no producer sets and no dispatcher handles | grep the value → only the type + a comment |
| 🔄 **Split meaning of one thing** | the same fact stored in N registries with N field names/units (drift) | compare field names + units across registries, show a real stale example |
| 🧹 **Partial-use borrow** | a heavy library imported, but only a tiny non-core surface used (full editor for 1 screen + 2 utility funcs) | list import lines vs the 1–2 actual call-sites |

**Governance method (for the report — how to keep it converged going forward):**
- **One concern = one seam.** For each of the enumerated concerns, name the *one* adapter that is canonical; every caller must go through it.
- **Dead means deleted.** An unwired adapter (dead-code signal) is either wired now or removed — never left as "reserved / pilot / test-only". A non-CI script referencing it is not a reason to keep it.
- **One metadata registry per concern.** Capability facts (context, modalities, dialect, limits) live in exactly one place; the other registries either read it or are merged/deleted. Never two names / two units for one fact.
- **Converge, don't keep both.** When a concern has 2 live adapters, decide "which is canonical" and migrate — do NOT keep N "because both work". N working adapters is still N× the maintenance.
- **Brace against future borrows**: the report's conclusion must state "the next time a tech is borrowed, it either enters the canonical seam or is deliberately scoped + removed; nothing stays 'borrowed but unwired'."

**Output**: list per concern → `concern | adapter cardinality (1 / N / 1-live-1-dead) | signal(s) marked | evidence file:line | canonical seam to converge to | action (delete / wire / merge)`.

> Every conclusion obeys iron rule 9: a convergence finding must be falsifiable — "delete the dead adapter → does behavior change? no → it was dead"; "route all callers through the canonical seam → do the N ways collapse to 1? yes → convergence confirmed".

### Step 10: Experience-layer three-way alignment + reference group

- **Three-way alignment**: for each feature, lay out ① design intent ② actual code behavior ③ real user expectation; find the gaps, focusing on "self-consistent but experience-wrong".
- **Reference-group experiment**: for features where "is the requirement itself right" is doubtful, design A/B comparisons, and on paper traverse what each design gives the user.

### Step 11: God's-eye global attribution (the last step — after all errors have arrived)

**This is the final attribution of the whole audit — wait until all errors from every prior step (walker traversal + six-layer checks + three-way alignment + stress testing) are collected.**

**Attribution methodology (distinguish "correlation" from "causation"):**

1. **Collect the full error set**: aggregate all errors into one table (each with file:line + trigger condition + symptom).
2. **Cluster candidate root causes**: by "shared code location / trigger condition / mechanism". Each cluster is a *candidate* (not yet verified).
3. **Counterfactual verification (separates root cause from symptom)**: for each candidate — "**if I fix this one spot, do the errors in this cluster disappear together?**" Disappear together → true root cause; only one disappears → it was a symptom. Write it out: `candidate X → assume fixed → traverse: do A/B/C disappear?`
4. **Draw the causal chain (not a flat list)**: `root cause → intermediate cause → symptom`. Mark necessary/sufficient conditions.
5. **Layer root causes + rank by impact**: prioritize by "how many symptoms one root cause eliminates", not by individual symptom severity.
6. **Every root cause carries a falsifiable verification method (mandatory)**: "how to verify it's true" — `assume X is true → fix X → expect A/B/C to disappear together; if they persist, X is wrong`. No verification path = tag `【impression, no verification path】`, never a conclusion.

### Step 12: Produce the report — classified layers + converged directions

Output the layered results + step 11's attribution. The body is the layer classification, but the **conclusion is "the few root causes + impact-ranked convergence directions"**, not a symptom list.

---

## 3. Output Format

```
# Persona Auditor Report
## 0. Audit tier + scope (which tier ran, what was skipped and why — honest)
## 1. User-persona layering table + design-intent table (source-tagged)
## 2. State-machine recovery + deviations from design intent
## 3. Project structure map (units × journeys inventory; coverage check)
## 4. Exhaustion matrix (persona × journey × operation × state × timing, coverage count)
## 5. Two-layer persona traversal results
     - 5a. Mechanical walker report: factual breaks (reached / broke / gate_or_break / steps / file:line)
     - 5b. Intent observer hypotheses: 【主观参考】opinions anchored to breaks
## 6. Extreme-test results — jailbreak / concurrency
## 7. Specialized-check results (security / AI-smell / compliance / general)
## 8. Release-hygiene check
## 9. Defensive-content pollution (five surfaces)
## 10. Wheel-reinvention audit
## 11. Adapter-convergence audit (tech-stack unification) — per concern: cardinality / signals / evidence / canonical seam / action
## 12. Experience-layer three-way alignment + reference group
## 13. God's-eye global attribution (counterfactually verified root causes)
      - full error set → candidate clustering → counterfactual verification → causal chain → impact ranking
## 14. Convergence directions (by root-cause impact, not symptom severity)
```

**Report header states the audit tier** and what was skipped (honest, not hidden). Every defect uniformly carries: `trigger sequence → expected vs actual → root → owning layer → severity → falsifiable verification`. The conclusion is "the few root causes + impact ranking", not dozens of symptoms laid flat.

---

## 4. Capability Boundary

**Capability list (seven dimensions)**:

| Dimension | What it detects |
|---|---|
| 🧠 Logic & state | logic contradictions, dead branches, deadlock, infinite loops, state pollution, stale state, missing preconditions, variables overwritten, checks skipped, race conditions |
| 👤 User experience | the "self-consistent but experience-wrong" gap, cognitive overload, feature fragmentation, missing feedback, "says vs does" mismatch, discoverability, recoverability, trust, mental-model mismatch, abandonment points |
| 🔒 Security | SQL injection, XSS, SSRF, IDOR, privilege escalation, command injection, path traversal, arbitrary file read, races, prompt injection, jailbreak (34 classes) |
| 🤖 AI-smell & maintainability | naming emptiness, comment clichés, over-abstraction, swallowed exceptions, TODO graveyard, over-engineering, reinvented wheels, outdated tech, **dead code / speculative abstraction**, **duplicate definitions / same-name conflicts**, **god files / over-centralized modules** |
| 📋 Compliance & release | software-copyright compliance, store-review gating, missing EULA/copyright notices, secret leakage, real PII leakage, internal traces, **defensive-content pollution** (corrections / warnings / discipline / self-reminders in public artifacts) |
| ⚡ Concurrency & stress | multi-task concurrency, races, timeouts, interruption, duplicate submission, rapid switching, multi-subagent conflicts |
| 🔗 Cross-scenario | cross-scenario contamination, precise switching, memory/state recall correctness, cross-module routing |
| 🧩 Tech-stack unification | **adapter-convergence failure**: one concern solved by N parallel adapters / 1 live + 1 dead / write-only capability registry / ghost enum value / two names or two units for one fact / partial-use heavy borrow |

> On-demand triggering, not a dump: ask "who is it for, where does it ship" and skip what doesn't apply (no store review for something not shipping, no mobile review for a desktop app).

- **Good at**: logic contradictions, stuck flows, state pollution, missing branches, path boundaries, experience-layer gaps, jailbreak penetration, concurrency races, AI smell, over-engineering, store rejections, **release hygiene**, **defensive-content pollution**, **wheel reinvention**, **adapter convergence (tech-stack unification)**.
- **Read-only during audit, fixing happens in the loop**: don't modify code during the audit (avoid contaminating the scene); the audit produces "a few root causes + precise file:line + falsifiable verification"; after user confirmation, the same agent fixes precisely and verifies "did the symptom disappear". The full loop is `audit → confirm → fix → verify`.
- **Limitation**: paper traversal surfaces specification-layer contradictions; if actual runtime behavior differs from what code implies, flag the gap and feed it back into the state machine.
- **Cannot do**: read binaries / reverse-engineer, replace real UI visual inspection, replace real execution regression (the latter go to vision tools / ux-toolkit / agent-qa). This skill is a "localization + big-picture + orchestration + differentiated-check" tool; fixing is executed by the agent after user confirmation.

---

## 5. When to Use, When Not

- **Use**: development has gotten fuzzy, repeatedly stuck in the first few steps, needs acceptance, before packaging / store submission / release, needs to judge "is the requirement itself right", needs a comprehensive layered audit, **or wants to plug into an existing dev loop (incremental / gate)**.
- **Don't use**: needs real UI visual inspection, needs real execution regression, pure performance benchmarks, wants only a single dimension (call the corresponding specialized skill directly).
