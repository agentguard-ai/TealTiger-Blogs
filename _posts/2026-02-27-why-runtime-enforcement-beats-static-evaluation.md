---
title: "Why Runtime Enforcement Beats Static Evaluation"
description: Offline evaluations are necessary—but insufficient. In agentic AI systems, real risk emerges at runtime, not in test suites.
date: 2026-02-27 14:00:00 +0530
tags: [runtime, evaluation, governance, agents, security]
---

# Post #6 — Outline

## 1. Executive summary (TL;DR)
- Static evaluations catch known failure modes, not emergent ones
- Agentic risk is contextual, multi-step, and time-dependent
- Runtime enforcement is where governance actually works
- Evaluations should *inform* policy, not replace it

---

## 2. The industry’s comfort zone: static evaluation
- Why evals feel safe and measurable
- Typical forms: prompt tests, red-teaming, benchmark suites, golden datasets
- Where evals shine (regressions, model comparison, prompt changes)

---

## 3. The hard limit of static evals
- Evaluations freeze context in time
- They cannot account for:
  - tool availability changes
  - user intent drift
  - data sensitivity discovered mid-execution
  - retry and loop dynamics
  - cost escalation

Key claim:
> Passing evals does not imply safe runtime behavior.

---

## 4. Why agentic systems break evaluation-first thinking
- Agents plan, act, observe, and replan
- Risk compounds across steps
- A safe step 1 does not guarantee a safe step 5
- Most incidents emerge *between* steps, not at entry

---

## 5. Where real failures happen (runtime-only failure modes)
- Tool misuse despite safe prompts
- Data egress triggered by intermediate context
- Budget overruns caused by retries and escalation
- Privilege creep through tool chaining

---

## 6. Runtime enforcement: the missing control plane
- Decision boundaries as enforcement points:
  - before tool calls
  - before output egress
  - before retries / escalation
- Deterministic outcomes: allow / deny / redact / approve / degrade

---

## 7. How evaluations should be used correctly
- Evals generate signals, not guarantees
- Feed eval findings into:
  - policy rules
  - thresholds
  - deny conditions
  - approval gates
- Continuous loop: eval → policy → runtime decision → audit → refine

---

## 8. A simple mental model

```text
Offline evals answer: "Could this go wrong?"
Runtime enforcement answers: "Can this happen right now?"
```

Both are required, but they solve different problems.

---

## 9. Adoption path (practical guidance)
- Start with evals to understand risk surface
- Add runtime logging to observe decisions
- Introduce log-only enforcement
- Graduate to enforced policy at high-risk boundaries

---

## 10. Common anti-patterns to avoid
- Treating eval pass rates as safety metrics
- Blocking releases solely on static scores
- Assuming prompt safety generalizes
- Relying on dashboards instead of gates

---

## 11. Related docs (TealTiger)
- Policy modes
- Validation and verification
- Audit and redaction
- Logging behavior
- Security vs governance

---

## 12. Closing thesis

Static evaluation reduces uncertainty.

> **Runtime enforcement reduces impact.**

In agentic AI, only one of those can stop an incident.
