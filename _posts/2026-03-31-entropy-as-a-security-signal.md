---
layout: post
title: "Entropy as a Security Signal: Detecting Exfiltration and Prompt Injection Without Training Data"
description: Entropy and distribution shift can act as lightweight, model-agnostic security signals—useful for early warning, not as proof.
tags: [foundations, governance, runtime, security-theory, agents]
category: governance

series: foundations
series_order: 3
status: design-notes
---

> **Status note:** This post explores scientific foundations that can improve auditability and correctness in AI security systems. The mechanisms described are **design direction** and may not be implemented in current TealTiger releases.

---

## Why we need signals that don’t depend on known patterns

Most AI “safety” detection still assumes the world is made of recognizable patterns:

- known jailbreak phrases
- known prompt‑injection templates
- known PII formats
- known malware‑like strings

Those approaches can help, but agentic systems fail in a different way: **the first time you see a new attack, it rarely looks like the last attack**.

What we want in production is a complementary capability:

> **A model‑agnostic sensor that can say “something is *different*” before we can say “this is exactly what it is.”**

Information theory provides a useful set of instruments for that job.

---

## Entropy in one sentence

**Entropy** measures uncertainty.

- Higher entropy → a signal looks more random / less predictable
- Lower entropy → a signal looks more repetitive / more predictable

In AI governance, we’re not using entropy as a magic detector. We’re using it as a **cheap, explainable indicator** that something in an agent’s behavior stream may have shifted.

---

## Where entropy shows up in agentic systems

In agentic systems, you have multiple observable streams:

1. **Inputs** (user messages, retrieved context, tool outputs)
2. **Tool use** (which tools, how often, in what sequences)
3. **Outputs** (generated content, structured results)

Entropy can be measured over each stream.

### 1) Input entropy (context flooding vs structured work)
- Context flooding often shows up as:
  - repeated characters
  - repeated instructions
  - long low‑information padding
- These patterns produce **abnormally low entropy**.

### 2) Tool‑call distribution entropy (collapse and loops)
A healthy workflow usually uses a mix of tools.

- A looping or stuck agent often concentrates calls into one tool or one action type.
- That concentration reduces **tool distribution entropy**.

This gives you a cheap early signal for:
- retry storms
- non‑converging plans
- tool‑call loops

### 3) Output entropy (structure collapse and exfil formats)
Outputs can shift in both directions:

- **High entropy** can correlate with:
  - encoded payloads
  - compressed/obfuscated data
  - random‑looking exfil formats
- **Low entropy** can correlate with:
  - rigid repeated templates
  - policy‑evasion through consistent formatting
  - low‑variance “laundering” outputs

The key point:

> Entropy does not tell you *what* is wrong. It tells you where to look.

---

## Beyond entropy: distribution shift (KL divergence)

Entropy is a snapshot. Often you also want:

> **How different is “today” from the baseline?**

That’s a distribution shift question.

A practical instrument here is **KL divergence**, which measures how one probability distribution differs from another.

In governance terms:

- Compare tool usage distribution this hour vs last week’s baseline
- Compare output token class distribution this task vs the agent’s normal profile
- Compare action mix distribution this run vs the workflow’s reference model

KL divergence is useful because it is:
- model‑agnostic
- measurable from telemetry
- explainable as “distance from baseline”

---

## What this enables (practically)

Used correctly, entropy and distribution shift become **signals** that feed governance decisions.

### Early warning for prompt injection attempts
Prompt injection often changes the shape of behavior:
- sudden tool expansion
- unusual outputs
- repeated retries

Entropy/KL can flag a *shift* even before you can classify the specific injection template.

### Exfiltration suspicion (encoded payloads)
If output becomes unusually random‑looking, entropy can prompt:
- stricter content inspection
- approval gating
- channel restrictions

### Looping and retry storms
Tool‑call distribution entropy can detect:
- “stuck” behavior
- repeated denies → retries
- runaway loops that inflate cost and exposure

### Context flooding
Input entropy can help detect:
- low‑information stuffing
- repeated control sequences
- attempts to bury policies in noise

---

## How TealTiger should use these signals (and how it should *not*)

This is the most important part.

### ✅ The safe pattern: signals escalate scrutiny
Entropy/KL should be used to:

- tighten policy mode (monitor → enforce → strict)
- trigger secondary checks (DLP, redaction, approval)
- narrow tool scope temporarily
- rate‑limit a run that shows collapse

### ❌ The unsafe pattern: signals become proof
Entropy should **not** be marketed as:
- a guarantee against prompt injection
- a definitive exfil detector
- a compliance control by itself

Reason: adversaries adapt.

They can:
- lower entropy exfil (structured tables)
- mimic baseline distributions
- split payloads across steps

So the correct positioning is:

> **Entropy and KL divergence are lightweight sensors that improve detection coverage when pattern-matching fails—but they are not enforcement proofs.**

---

## Evidence implications (why this matters for auditability)

Even as signals, entropy/KL can be very audit-friendly if logged correctly.

A good evidence record would capture:

- the baseline window used (time range, sample size)
- the observed value (entropy, KL divergence)
- the threshold that triggered escalation
- the resulting action (throttle, approval, restrict tool)

This is important because it turns “the system felt weird” into:

- **a measurable anomaly**
- **a traceable decision**
- **an explainable escalation**

---

## What’s next in the series

Signals are useful—but behavior unfolds over time.

If you want to detect:
- loops,
- rare transitions,
- escalation sequences,

you need to model **trajectories**, not individual events.

That leads to **Markov models** for agent behavior: a simple, practical way to quantify what “normal” sequences look like and flag what deviates.

---

*This post is part of the* **Toward Provable AI Governance** *series.*
