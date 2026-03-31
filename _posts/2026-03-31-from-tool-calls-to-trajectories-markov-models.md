---
layout: post
title: "From Tool Calls to Trajectories: Using Markov Models to Catch Agent Loops and Escalations"
description: Single events rarely explain agent risk. Markov models make sequences measurable—highlighting loops, rare transitions, and escalation paths in audit-friendly ways.
tags: [foundations, governance, runtime, security-theory, agents]
category: governance

series: foundations
series_order: 4
status: design-notes
---

> **Status note:** This post explores scientific foundations that can improve auditability and correctness in AI security systems. The mechanisms described are **design direction** and may not be implemented in current TealTiger releases.

---

## Why single events don’t capture agent risk

Most AI safety checks still operate on **single events**:

- a prompt contains a suspicious phrase
- a response contains PII
- a tool call hits a forbidden endpoint

Those checks matter, but agentic systems fail in a different way.

In production, many failures are **trajectory failures**:

- repeated deny → retry → deny → retry loops
- gradual scope expansion across tool calls
- rare state jumps (read → admin-write) that are innocuous alone but dangerous in sequence
- non‑converging plans that inflate cost and exposure

If we want governance that holds under real workloads, we need to treat agent behavior as what it is:

> **A sequence of states over time, not a single message.**

That is exactly what Markov models help with.

---

## Markov models in one sentence

A **Markov chain** models a system that transitions between states, where the next state depends primarily on the current state.

In practical terms:

- Define a small set of **states** (e.g., `plan`, `tool_call`, `policy_denied`, `output`, `retry`)
- Observe transitions (e.g., `plan → tool_call`, `tool_call → output`)
- Learn transition probabilities from telemetry
- Flag **rare transitions** or **unstable cycles**

You don’t need the model to be perfect. You need it to be **consistent**, measurable, and explainable.

---

## How to define “states” for an agent runtime

In governance, the most important step is making states **operationally meaningful**.

A good state taxonomy is:

- small enough to be stable
- expressive enough to capture risk
- derived from telemetry you already collect

### Example state space (practical)

- `plan`
- `retrieve`
- `tool_call:read`
- `tool_call:write`
- `tool_call:admin`
- `policy_allowed`
- `policy_denied`
- `approval_requested`
- `approval_granted`
- `output`
- `retry`
- `terminate`

This taxonomy makes governance questions expressible as measurable sequences.

---

## What Markov models enable (concretely)

### 1) Loop detection (non‑convergence)

Agents often fail by looping.

A Markov model makes loops visible as:

- high probability cycles (`plan → tool_call → plan → tool_call`)
- repeated transitions into `retry`
- unusually long expected time to reach terminal states (`output` or `terminate`)

Governance actions that can follow:

- throttle retries
- require approval after N loops
- degrade model / reduce tool scope
- trip a circuit breaker based on expected non‑convergence

### 2) Escalation detection (rare transitions)

Many dangerous behaviors are **rare state jumps**.

Examples:

- `tool_call:read → tool_call:admin`
- `policy_denied → tool_call:write` (attempt to bypass)
- `approval_requested → tool_call:admin` (acting before approval)

A Markov model can flag these because they have low probability under the baseline.

Governance actions:

- upgrade to strict mode
- require approval gating
- deny specific tool classes temporarily

### 3) Hijacked agent detection (distribution drift in transitions)

A compromised agent often doesn’t fail instantly.

Instead, the transition structure changes:

- more retries
- more denials
- tighter cycles
- tool usage becomes concentrated

Transition drift can be detected by comparing:

- today’s transition matrix vs baseline
- steady‑state distribution shift (e.g., spending too much time in `retry`)

---

## A minimal, audit-friendly implementation sketch

You can keep this implementation extremely simple.

### Transition count → probability

- Maintain counts `C[from][to]`
- Convert to probabilities with smoothing:

`P(to | from) = (C[from][to] + α) / (Σ_k C[from][k] + α·|states|)`

Where `α` is a small constant to avoid zero-probability issues.

### Anomaly scoring

For a new transition `from → to`:

- compute `P(to | from)`
- flag if `P < threshold`

Or compute a trajectory score:

- sum of `-log P(transition)` across the run
- alert if above a threshold

This creates an explainable story:

> “This run contained three rare transitions with probability < 0.5% under the baseline.”

---

## How this fits TealTiger’s philosophy (deterministic-first)

Markov models are **statistical**.

That means they should not become the primary enforcement oracle.

The safe posture is:

- **Deterministic controls** fail-closed where required (data flow, approvals, budget caps)
- **Markov signals** escalate posture (monitor → enforce → strict) and trigger secondary checks

In other words:

> Markov models tell you *where the behavior is strange*.
> Deterministic policy tells you *what must never happen*.

---

## Evidence implications (what to record)

Markov-based monitoring can be very audit-friendly if you record the right artifacts.

A defensible evidence record should include:

- the state taxonomy version (so “state” meaning is stable)
- baseline window and sample size
- transition matrix hash (or version)
- anomalous transitions (from, to, probability)
- trajectory score for the run
- resulting governance action (throttle, approval, deny)

This converts “the agent behaved oddly” into:

- a measurable anomaly
- a reproducible model reference
- an explainable intervention

---

## What’s next in the series

Markov models help you detect instability and escalation in sequences.

The next question is:

> If systems can suddenly shift from stable to unstable, can we detect that shift *before* it cascades?

That leads into **tipping points and catastrophe-aware circuit breaking**—how to recognize early warning signals of collapse in cost, retries, and error rates.

---

*This post is part of the* **Toward Provable AI Governance** *series.*
