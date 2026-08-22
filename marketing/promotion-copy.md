# Promotion Copy · Persona Auditor

按平台取用。

---

## 核心钩子（一句话，所有平台的锚点）

> AI 写的代码**能跑**，但你的用户一用就懵。它从真实用户的眼睛帮你把 bug 和根因一起找出来——**一次修对，而不是改二十次**。

---

## ① X / Twitter（英文，短）

```
AI code that *runs* ≠ code that *works*.

Your users keep getting stuck, and you keep patching without finding why.

Persona Auditor audits from real users' eyes:
• digital personas walk every journey
• finds bugs you'd only discover by expensive trial-and-error (or never)
• collapses them into 2–3 root causes

Fix once. Not 20 times.

npx skills add kevinshi3200/persona-auditor
```

---

## ② Reddit / HN（英文，结构化）

**Title:** I built a skill that finds the bugs your AI-written code doesn't know it has

**Body:**

You know the loop: AI ships code that passes tests and *looks* correct, but a real user gets stuck in the first three steps. You patch one thing, they hit another, and after a week you still can't name the actual root cause.

I built **Persona Auditor** to break that loop.

What it does differently:

- **It audits as a real user, not as a linter.** Digital personas (subagents playing your actual user types) walk every user journey and predict what a person would do, see, and misunderstand — not whether the code type-checks.
- **It finds what you'd never notice.** "Self-consistent but experience-wrong" bugs: stale state, cross-project contamination, feedback that says one thing and does another.
- **It gives you root causes, not a wall of symptoms.** God's-eye counterfactual attribution collapses dozens of issues into 2–3 root causes, each with `file:line` and a falsifiable way to verify the fix.
- **It's exhaustive, not sampled.** It covers the full persona × feature × operation matrix — not one happy path.
- **It catches AI-specific leaks.** Two checks nothing else does: *release hygiene* (secrets, PII, and your dev trial-and-error traces accidentally committed) and *wheel-reinvention* (modules you hand-rolled that a mature library already beats).

Works with Claude Code, Codex, and Cursor:

```
npx skills add kevinshi3200/persona-auditor
```

Happy to answer questions in the comments.

---

## ③ V2EX / 知乎（中文，结构化）

**标题：** AI 写的代码能跑，但用户一用就卡——我做了个 skill 专治这个

**正文：**

你有没有过这种循环：AI 生成的代码测试全过、逻辑看着没毛病，可真实用户一上手，头三步就卡住。你改一处，他撞另一处，改了一星期，还是说不出根因在哪。

我做了个 **Persona Auditor**，专门破这个循环。

它跟 lint、单测、code review 不是一回事，核心区别就三点：

1. **它用「真实用户」的眼睛看代码，不是用机器。** 它派「数字人」——每个扮演你的一类真实用户，把整条用户旅程从头走到底，预测真人会在哪一步卡住、误解、放弃。代码类型对不对是次要的，**人会不会用错**才是它抓的。
2. **它找的是你「意识不到」的 bug。** 不是那种报错就能看见的，而是"代码自洽但体验不对"：状态残留、跨项目串台、说一套做一套。
3. **它给你根因，不是几十条症状。** 上帝视角反推，把几十个 bug 归成 2~3 个根因，每个都带精确 `文件:行号` 和"怎么验证修好了"。**一次修对，而不是改二十次。**

另外还有两个别人没有的独家检查：

- **发布卫生**：AI 开发特有的病——把密钥、真实个人信息、开发时的试错痕迹写进了代码，上线就漏。
- **造轮子审计**：你手写了一个模块，其实早有更成熟的开源库，白费维护成本。

Claude Code / Codex / Cursor 一行装上：

```
npx skills add kevinshi3200/persona-auditor
```

有问题欢迎在仓库 issue 里聊。

---

## ④ 一句版（README 顶部 / 别人问"这是啥"时）

> **Persona Auditor** —— 从真实用户视角审计 AI 代码，找出你反复试验才能发现、甚至永远发现不了的错误，归因成少数根因，一次修对。
