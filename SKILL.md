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
  - software copyright compliance
  - store review
  - secret leakage
  - release hygiene
  - cross-project contamination
  - dynamic memory pool
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
  - 跨项目串台
  - 动态记忆池
---

# Persona Auditor（数字人审计员）

## 1. Positioning & Iron Rules

**Positioning**: a **comprehensive audit-orchestration skill** whose goal is **precise fixing** — find the errors the user would only discover through costly trial-and-error (or never notice at all), collapse them into a handful of root causes, and let the agent fix it right the first time. Three legs:

1. **Main line (the differentiator)**: user personas → state-machine exhaustion → persona simulation → experience-layer three-way alignment → reference group → god's-eye attribution. It hunts the "contradiction between underlying logic and user experience" — built for "stuck on one point across many rounds of fixes, missing the global view".
2. **Orchestrated reuse (don't reinvent)**: for security / AI-smell / over-engineering / store review / general correctness, load and run existing skills and merge their results into one unified report.
3. **Two self-built checks (not covered by any existing skill)**:
   - **Release hygiene**: an AI-assisted-development disease — development trial-and-error, conversations, summary conclusions, real personal information, secrets, and internal decisions written into code / config / comments / docs.
   - **Wheel-reinvention audit**: find "this whole module has a more mature open-source alternative, but you hand-rolled an outdated one" — search and compare, then ask the user whether to replace.

**Paper traversal is the means, not the end**: not executing, not clicking is how we *cheaply exhaust every user path* — faster, more global than real clicking, able to see root causes at a glance. **But it is never an excuse to "not fix"**: the audit's closed loop is `report (root cause + precise location) → user confirms → agent fixes precisely → verify via falsifiable check (symptom disappears)`.

**Iron rules**:
1. **Do not execute code, do not really click, do not modify code** — mainline work happens on paper.
2. **Code is the single source of truth**: reason from the real behavior recovered by reading code, not from design docs or memory.
3. **Do not fall into the "code self-consistency" trap**: internally consistent code ≠ a good product.
4. **Exhaustiveness is the soul; sampling is negligence**: steps 2/3 must fully traverse the `persona × function × operation × state × timing` matrix; many persona subagents each reason independently; **only after traversing all paths may you do root-cause analysis**. Forbidden to sample one happy path and conclude.
5. **Reuse first, don't reinvent**: for security / AI-smell / maintainability / review, load `llm-sast-scanner` / `code-auditor` / `ponytail-audit` / `code-reviewer`; do not rewrite their rules.
6. **Release hygiene and wheel-reinvention audit are this skill's own checks — always run them, never skip.**
7. **Read-the-code verification spans multiple steps, not just once at step 1**: persona simulation (step 3), extreme testing (step 4), three-way alignment (step 8), and attribution (step 9) must each **re-read the code to verify**; forbidden to conclude from step-1 memory. Every `file:line` in a conclusion must be "just read / just grepped in this round", not "from memory" — memory goes stale and hallucinates; only freshly read evidence counts.
8. **False-positive check (mandatory before reporting)**: before reporting anything, confirm "does this scenario apply?" — don't report store review for something not being shipped, don't report software-copyright compliance for personal use, don't report mobile App Store review for a desktop app, don't treat test fixtures as production bugs. **A wrong report hurts trust more than a missed one.**
9. **What not to touch / not to report**: test fixtures / vendored third-party code (minified) / build artifacts (out/dist) / .env templates / TODO placeholders in docs — these are **not problems** by default, unless the user explicitly asks to audit them. Reporting them is a false positive.
10. **If you can't find it, say honestly what's missing — never fabricate.**
11. **Conclusions must be falsifiable (the bottom line of being scientific)**: every root-cause / defect conclusion must carry a **falsifiable verification method** — "if I fix X, symptom Y should disappear; if Y persists after fixing X, this conclusion is wrong." **Forbidden to report unfalsifiable conclusions** (e.g. "bad architecture", "poor code quality", "not good enough"). A conclusion with no verification path = unfalsifiable = not an audit finding, only an "impression" — downgrade it and tag `【impression, no verification path】`.

---

## 2. Methodology (Intake Gate + Ten Steps)

### Intake Gate (mandatory, never skip, never guess)

The "ruler" of paper traversal is only known to the user — **the agent must never make it up**.

**Step 1: classify the project.** Quickly read `README` / `package.json` / directory structure to determine the project type (web app / CLI / library / desktop app / agent system / game / other).

**Step 2: four universal questions** (ask for any project):
1. **Who is it for**: how many target user types? What usage habits, expectations, and tolerance does each have?
2. **Design intent**: for each core feature, what experience did the designer want the user to get?
3. **Release scenario (decides whether compliance / store-review checks apply — not asking this guarantees false positives)**: where does it ship (App Store / Google Play / domestic app stores / direct download / not shipped)? Personal or commercial? Is a software copyright registration (软著) planned? — **This directly decides whether "software-copyright compliance" and "store-review gating" checks run**: if not shipping / personal use / no registration, skip these checks — otherwise everything is a false positive.
4. **Audit scope**: all six layers, or only some?

**Step 3: type-specific follow-ups** (ask targeted questions after classification, so the intake gate produces correct, useful questions for any project):
- **web / desktop app**: which user journeys are core? Which step most easily loses / confuses users? Any target platform to ship to?
- **CLI / library**: who are the target developers? Typical integration scenarios? Any API constraints deliberately maintained?
- **agent system**: human-computer interaction mode (chat / canvas / voice)? Where is the automation boundary (which actions must never be auto-run)?
- **game**: target players? Core fun? Any payment / store plan?

**Fallback rule**: for items the user cannot answer, reverse-engineer from `code + README + design docs`, and tag in the report `【reverse-inferred, not user-stated, needs confirmation】`. During the audit, if a point arises that "only the user can decide" (which A/B reference group to choose, whether the requirement itself is right, whether to replace a reinvented wheel), **stop and ask the user — don't decide for them**.

### Light Research (after intake gate, before step 0, mandatory)

**Personas must not be invented off the top of your head — do a round of light research first to calibrate personas against real data, then choreograph simulated behavior.** This is not deep research; keep it to a few searches, only to get three things:

1. **Target-user commonalities**: search real user profiles for this kind of product/scenario (web_search / communities / app-store reviews / competitor reviews), extract "what kind of people these users generally are, and their habits".
2. **Real pain points**: what do these users commonly complain about, where do they get stuck, what hurts most.
3. **Behavior patterns**: how do these users typically use it (one-off? high-frequency? multi-task switching?).

**Three disciplines**: ① every research conclusion **carries a source URL**; ② only search "commonalities + pain points + behavior patterns", don't sink into massive searching (light research = capped at 3–5 searches); ③ when research conflicts with the user-stated intake answers, **user-stated wins** (the founder knows their product better than external research), but list the conflict explicitly for the user to decide.

### Step 0: Set the ruler — user-persona layering (targeted)

Based on a **three-way synthesis**: intake gate (user-stated) + light research (external real-user data) + reverse-inference (code/docs), produce:

- **User-persona layering table**: ≥3 user types; for each write: usage habits / expectations / tolerance / features they will and won't use / **pain points** / **behavior patterns**. Tag each item's source: 【user-stated】/【light research · URL】/【reverse-inferred】.
- **Design-intent table**: for each feature, write "what experience the designer wanted the user to get", with source tags.

### Step 1: Recover the truth — read code and model the state machine

- Recover the **real state machine**: state variables (with initial values), transition conditions, branches, boundaries, race points.
- Recover the **real behavior of each feature** (what the code actually does, not what the docs claim).
- Annotate: deviations between actual code behavior and design intent.

### Step 2: Exhaust paths — persona combos + combination-reuse principle (exhaustion ≠ full Cartesian product)

**Iron rule: exhaustion = fully traversing `persona combos × function × operation × state × timing`, but "combos" use equivalence classes + boundary values + orthogonal arrays, not the full Cartesian product. Forbidden to sample one happy path and conclude.**

**The no-miss methodology (the classic software-testing trio, guaranteeing "no miss + no explosion")**:

1. **Equivalence-class partitioning**: partition each dimension into equivalence classes first — personas by "behavioral similarity" (not infinite subdivision); features grouped by "same kind of behavior" (all "create card" is one class, no need to test each card separately); operations into "normal / abnormal / boundary / interruption" four classes, pick representative values per class (one representative each for empty input, overlong, out-of-order — don't test character by character).
2. **Boundary-value analysis**: for numeric / length / count boundaries, test **both sides** (0 and 1, upper-bound and upper-bound+1, empty and 1 char) — bugs cluster at boundaries.
3. **Orthogonal arrays (reduce dimensions without missing)**: don't test the full Cartesian product; use an orthogonal table to guarantee "**every pair of dimensions' combination is covered at least once**". Use only for 3+ factors (persona × function × operation); for 2 factors, do the full combination.

**Coverage must be quantified and provable**: after exhaustion, the report must state — `covered N persona types × M function groups × K operation classes = P paths; the orthogonal table guarantees every two-dimension combination is covered at least once; boundary values cover 0/1/upper/upper+1`. **"No miss" is not an empty claim — it is guaranteed by the mathematical properties of equivalence classes + boundary values + orthogonal arrays, and the coverage count must be written out.**

**Persona = persona × scenario combination** (not a single persona, and not the full persona×scenario Cartesian product):

1. First list **single-scenario personas**: each persona type × each scenario (e.g. novice × vibe coding, novice × comic drama, expert × vibe coding, expert × comic drama…).
2. Then list **combined-scenario personas**: real users often "do both A and B" (e.g. a novice does both vibe coding and comic drama).
3. **Combination-reuse principle (key, avoids combinatorial explosion)**:
   - If "doing A alone" and "doing B alone" are each tested and fine, then the "does both A and B" combined persona **does not retest everything** — it only tests the **cross-scenario delta**:
     ① **precise navigation**: is switching between A↔B precise, reversible, and free of leftover wrong state;
     ② **cross-project memory recall**: does the memory pool know the relationship between A and B, and can it answer "I previously did X in project A";
     ③ **is the dynamic memory pool working**: is cross-project memory recalled correctly, without contamination or loss.
   - Only fully test a combination when "a single scenario itself wasn't tested" or "the combination introduces new state".

**Persona count is justified, not arbitrary**: total personas = number of needed persona combos, **capped at ≤10** (saves tokens + matches the agent's max concurrency). Over the cap, prioritize by "single-scenario first, then high-value combos", and state in the report which combos were dropped and why.

### Step 3: Deterministic persona simulation + god's eye (not chaotic simulation; token-controlled)

**Key distinction**: we do **deterministic traversal** — code is deterministic, the state machine is deterministic; this is not MiroFish-style "social evolution / emergent swarming" chaotic reasoning. So the persona count = step 2's **persona-combo count** (capped ≤10, not thousands), and tokens are controlled.

1. **Dispatch persona subagents (prompt has a basis, not invented)**: for each persona combo from step 2, dispatch one subagent (a "digital persona"). **The persona prompt must be synthesized from three sources**: ① light research (external real users' commonalities + pain points + behavior patterns, with URLs) ② intake gate (founder's design intent + target users) ③ code reverse-inference (feature list + state machine). Fixed prompt fields: `identity / usage habits / expectations / tolerance / pain points / behavior patterns / features they will and won't use / the path list this persona must traverse`. **Each persona reasons independently, without referencing others; combined-persona personas only test the cross-scenario delta, not re-testing already-covered single scenarios.**

2. **Each persona verifies along the full code chain, line by line**: for each of its paths, from `user action → entry function → state-machine transition → tool execution → persistence → output → new state`, **read the code at each link to verify the real behavior**, and record each step: `which branch was actually taken / how state changed / what was output / where it broke`. **Forbidden to flag a line just because it looks like a bug — you must walk the full chain before you may say "this path breaks at X".**

3. **Prediction + causality (predictions must close the loop to code evidence)**: each persona not only records "it broke", but also **predicts**: after this user completes this step, what will they do next, what will they see, what misunderstanding will they form. **Every prediction must land on "which line of code caused this outcome"** — a prediction without code evidence is tagged `【speculation, not localized】`; forbidden to pass off "storytelling" as "traversal".

4. **Personas also detect these dimensions (not just "does it run")**:
   - **Discoverability**: how does this user discover a feature? What if they can't find it?
   - **Recoverability**: can they undo/recover from a mistake? Is the error message understandable?
   - **Trust building**: why would the user believe the agent did it right? Where's the evidence/feedback?
   - **Mental-model consistency**: does what the user assumes match what the system actually does? (key — corresponds to "self-consistent but experience-wrong")
   - **Abandonment point**: at which step would this user give up / leave? Why?
   - **Privacy perception**: this user's perception and fear of "where does my data go".

5. **Make disagreements explicit**: different personas may judge the same path differently (novice feels stuck, expert feels normal, specialist feels too verbose) — **this disagreement itself is evidence of "experience-layer gap"**, and must be recorded explicitly as "which persona judged what, and why they disagree", never smoothed over.

6. **Aggregate persona results**: wait until **all paths of all personas** are traversed, then do a "persona-layer" aggregation — which breakpoints are single-user-specific (boundary), which are hit by all users (systemic), and where personas disagree. **Note: this is only aggregation, not global attribution — global attribution is step 9 "god's eye", done only after all errors (not just personas, but also the six-layer checks) have arrived.**

### Step 4: Extreme testing — jailbreak/injection + stress/concurrency

- **Jailbreak / injection**: prompt injection, role hijacking, system impersonation, jailbreak templates, invisible Unicode. Against step 1's "trusted input boundary", find penetration points and correct interception points.
- **Multi-task concurrency stress**: run N tasks simultaneously, same-card concurrency, races, timeouts, interruption, duplicate submission, rapid switching. Traverse who overwrites whom, whether locks fail, whether queues are serialized.

### Step 5: Specialized-check orchestration — reuse existing skills (don't reinvent)

Load and run the existing skills below, and merge their results back into the report:

| Dimension | Reused skill | What it checks |
|---|---|---|
| Security | `llm-sast-scanner` | 34 vulnerability classes, Source→Sink taint tracking |
| AI-smell / maintainability | `code-auditor` (AI-smell section) + `ponytail-audit` | AI naming emptiness / comment clichés / swallowed exceptions / TODO graveyard; over-engineering delete/stdlib/native/yagni/shrink |
| Store review | `code-auditor` (software-copyright + store-gating section) | **Conditional trigger**: only run when the intake gate's "release scenario" explicitly says it will ship / register software copyright; if personal use / not shipping / no registration, **skip this row** (otherwise all false positives). And know the platform first: an Electron desktop app does not go through App Store/Google Play mobile review — reporting "mobile store 4.2 / low-quality" is a false positive |
| General correctness | `code-reviewer` | correctness / security / maintainability / performance 🔴🟡💭 |

Duplicates go to the more specialized one; dedupe then merge. **Before reporting each layer's results, pass iron rule 8 "false-positive check": does this scenario apply? If not, don't report — tag "this layer skipped because the release scenario doesn't apply."**

### Step 6: Release-hygiene check (self-built differentiator, mandatory)

**AI-assisted-development-specific — things that should not be publicly released got written into code/config/comments/docs.** Scan item by item:

- **Real personal information**: name / phone / email / address / school / grades / ID number, hardcoded into code, config, fixtures, comments.
- **Secrets and credentials**: API key / secret / token / private key / personal workspace endpoint, written into code (especially outside .env).
- **Development trial-and-error traces**: internal process records in comments like "incident observed", "user said XX", "audit found XX", "walkthrough gap N", "temporary / do this for now / to delete".
- **Internal conversations and summary conclusions**: AI wrote development-time conversations, trial-and-error conclusions, internal decisions into prompts/comments/docs (e.g. "verify item by item at acceptance", "must dispatch subagents" — private intent).
- **Internal domain information**: internal project names, private domains, internal architecture decisions, unpublished account systems.

For each hit, tag `【release hygiene】+ file:line + content category + leak level`, and list it separately in the report. **This is not in any existing skill's scope — this skill covers it exclusively.**

### Step 7: Wheel-reinvention audit (self-built differentiator, mandatory)

**Find "this whole module has a more mature open-source alternative, but you hand-rolled an outdated one."**

1. Enumerate the project's feature modules / self-built dependencies (especially "looks hand-written": auth, state management, message queue, ORM, template engine, utility collections, protocol implementations…).
2. For each suspected self-built module, **search first** (web_search / npm view / GitHub search) for mature open-source alternatives, comparing: maturity, maintenance activity, feature completeness, license (commercially usable, non-infringing).
3. **Code comparison**: existing self-built implementation vs the mature alternative — where does it fall short (over-wrought? outdated tech? missing features? high maintenance cost?).
4. When something is "clearly worse than the mature alternative", **stop and ask the user whether to replace it** — don't decide unilaterally (the user may have a deliberate reason to self-build).

### Step 8: Experience-layer three-way alignment + reference group

- **Three-way alignment**: for each feature, lay out three things — ① design intent ② actual code behavior ③ real user expectation; find the gaps, focusing on "self-consistent but experience-wrong".
- **Reference-group experiment**: for features where "is the requirement itself right" is doubtful, design A/B comparisons, and on paper traverse what each design gives the user — to align requirements and cut communication cost.

### Step 9: God's-eye global attribution (the last step — only after all errors have arrived; scientific attribution)

**This is not the tail of persona simulation, it is the final attribution of the whole audit — it must wait until all errors from every prior step (persona simulation + six-layer checks + three-way alignment + stress testing) are collected, then take the whole picture in at once.**

**Attribution methodology (distinguish "correlation" from "causation"; don't just group similar-looking things):**

1. **Collect the full error set**: aggregate all errors found in prior steps into one "full error set" table (each with file:line + trigger condition + symptom).

2. **Cluster candidate root causes**: cluster by "shared code location / shared trigger condition / shared mechanism". Each cluster is a **candidate root cause** (note: at this point it's only a "candidate", not yet verified).

3. **Counterfactual verification (key — separates root cause from symptom)**: for each candidate root cause, do counterfactual reasoning — "**if I fix this one spot, do the errors in this cluster disappear together?**"
   - Disappear together → it's a **true root cause**;
   - Only one disappears, the rest remain → it's just a **symptom** (or "shared appearance"); the real root cause is elsewhere.
   - Write the counterfactual out: `candidate root cause X → assume X fixed → traverse: do A/B/C disappear as a result?`

4. **Draw the causal chain (not a flat list)**: `root cause → intermediate cause → symptom`. Multiple symptoms may trace to one root cause; one symptom may have multiple contributing causes — mark "necessary/sufficient":
   - **Necessary condition**: without it the failure doesn't happen (but having it doesn't guarantee it happens);
   - **Sufficient condition**: with it the failure always happens.

5. **Layer root causes + rank by impact**: prioritize by "how many symptoms one root cause eliminates" (impact), not by individual symptom severity. **The report ultimately highlights only a handful of root causes, not dozens of symptoms** — that's what "big-picture" means.

6. **Every root cause carries a falsifiable verification method (mandatory)**: each root-cause conclusion must spell out "**how to verify it's true**" — `assume root cause X is true → fix X → expect symptoms A/B/C to disappear together; if they persist after fixing, X is not the root cause and attribution must be redone`. This is the landing loop of attribution conclusions: audit is not the endpoint, verified fixing is. **Any root cause without a verification path is tagged `【impression, no verification path】`, and must not be treated as a conclusion.**

### Step 10: Produce the report — six-layer classification + converged directions after attribution

Output the six-layer results + step 9's attribution together. The report body is the six-layer classification, but the **conclusion section must be "the few root causes after attribution + convergence directions ranked by impact"**, not a symptom list.

---

## 3. Output Format (Six-Layer Classification)

```
# Persona Auditor Report

## 0. User-persona layering table + design-intent table (source-tagged: user-stated / reverse-inferred)
## 1. State-machine recovery + deviations from design intent
## 2. Exhaustion matrix (five dimensions: persona × function × operation × state × timing, fully traversed)
## 3. Multi-persona simulation results — logic layer (each persona reasoned independently + prediction causality, with per-path full-chain verification records)
## 4. Extreme-test results — jailbreak penetration points / concurrency race points
## 5. Specialized-check results
     - Security layer (llm-sast-scanner)
     - AI-smell / maintainability layer (code-auditor + ponytail-audit)
     - Compliance layer (code-auditor software-copyright + store gating)
     - General (code-reviewer)
## 6. Release-hygiene check — release-hygiene layer (this skill's differentiator)
## 7. Wheel-reinvention audit — architecture layer (this skill's differentiator, incl. "whether to ask about replacement")
## 8. Experience-layer three-way alignment + reference group — experience layer
## 9. God's-eye global attribution (root causes after counterfactual verification, not a symptom list)
     - Full error set (deduped)
     - Candidate root-cause clustering + counterfactual verification ("fix it, which symptoms disappear together")
     - Causal chain: root cause → intermediate cause → symptom (mark necessary/sufficient)
     - Root-cause impact ranking (how many symptoms one root cause eliminates)
## 10. Convergence directions (by root-cause impact, not by symptom severity)
```

Every defect uniformly carries: `trigger sequence → expected vs actual → root → owning layer → severity`. **The conclusion section must be "the few root causes after attribution + impact ranking", not dozens of symptoms laid flat.**

---

## 4. Capability Boundary

**Capability list (seven dimensions)**:

| Dimension | What it detects |
|---|---|
| 🧠 Logic & state | logic contradictions, dead branches, deadlock, infinite loops, state pollution, stale state, missing preconditions, variables accidentally overwritten, checks skipped, race conditions |
| 👤 User experience | the "self-consistent but experience-wrong" gap, cognitive overload, feature fragmentation, missing feedback, "says vs does" mismatch, discoverability, recoverability, trust building, mental-model mismatch, abandonment points |
| 🔒 Security | SQL injection, XSS, SSRF, IDOR, privilege escalation, command injection, path traversal, arbitrary file read, races, prompt injection, jailbreak (34 classes) |
| 🤖 AI-smell & maintainability | naming emptiness, comment clichés, over-abstraction, swallowed exceptions, TODO graveyard, over-engineering, reinvented wheels, outdated tech, worse-than-mature-OSS |
| 📋 Compliance & release | software-copyright compliance, store-review gating, missing EULA/copyright notices, secret leakage, real PII leakage, internal info / trial-and-error traces leaked |
| ⚡ Concurrency & stress | multi-task concurrency, races, timeouts, interruption, duplicate submission, rapid switching, multi-subagent conflicts |
| 🔗 Cross-project | cross-project memory contamination, precise navigation, dynamic memory pool, cross-domain routing |

> On-demand triggering, not a dump: before auditing, ask "who is it for, where does it ship", and automatically decide which dimensions apply and which to skip (not shipping → don't report store review; desktop app → don't report mobile app review) to avoid false positives.

- **Good at**: logic contradictions, stuck flows, state pollution, missing branches, path boundaries, experience-layer gaps, jailbreak penetration, concurrency races, AI smell, over-engineering, store rejections, **release hygiene (AI-specific)**, **wheel reinvention (mature-alternative comparison)**.
- **Read-only during audit, fixing happens in the loop**: during the audit, **don't modify code** (to not contaminate the scene, not affect running things), but the audit produces "a few root causes + precise file:line + falsifiable verification methods"; **after the user confirms, the same agent fixes precisely per the report, then verifies "did the symptom disappear"**. This is the complete `audit → confirm → fix → verify` loop; audit is only the first leg.
- **Limitation**: if the code's actual behavior differs from "the behavior you assumed", paper traversal can only surface "specification-layer contradictions". Mitigation: when you find "traversal ≠ reality", feed the reality back into the state machine.
- **Cannot do**: cannot read binaries / reverse-engineer (only read plain-source), cannot replace real UI visual inspection, cannot replace real execution regression (the latter two go to vision tools / ux-toolkit / agent-qa). This skill is a "localization + big-picture + orchestration + differentiated-check" tool; fixing is executed by the agent after user confirmation.

---

## 5. When to Use, When Not

- **Use**: the longer development goes the fuzzier things get, repeatedly stuck in the first few steps, needs acceptance, before packaging, before store submission, before release, needs to judge "is the requirement itself right", needs a comprehensive six-layer audit.
- **Don't use**: needs real UI visual inspection, needs real execution regression, pure performance benchmarks, wants only a single dimension (call the corresponding specialized skill directly).
