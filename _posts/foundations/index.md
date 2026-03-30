---
title: "Toward Provable AI Governance"
description: A single-page series hub that includes the kickoff and all posts (combined) for easy copy/paste and publishing.
tags: [foundations, design-notes, provable-governance, governance, security-theory, agents]
---

# Toward Provable AI Governance — Series Hub (Combined)

TealTiger is built around a simple premise: **security and governance must be enforceable at runtime and defensible through evidence**—not just encouraged through prompts.

This page is a **single combined hub** for the *Toward Provable AI Governance* series. It includes:
- A series overview
- A publish order
- The kickoff + intros for each post

> **Status note:** These posts are **design notes**. They discuss scientific foundations and architectural direction. **Not all mechanisms described are implemented in current TealTiger releases.**

---

## Recommended publish order

1. Toward Provable AI Governance: Why Agent Security Needs Math, Not Just Guardrails
2. Stop Guessing Policy Thresholds: A Game-Theoretic View of AI Guardrails
3. Entropy as a Security Signal: Detecting Exfiltration and Prompt Injection Without Training Data
4. From Tool Calls to Trajectories: Using Markov Models to Catch Agent Loops and Escalations
5. Why AI Systems Fail Suddenly: Tipping Points, Cascades, and Preemptive Circuit Breakers
6. Policies You Can Prove: Temporal Logic for Auditable, Trace-Based AI Safety
7. Beyond DLP Regex: Lattice-Based Information Flow Control for AI Agents
8. Trust That Updates: Bayesian Security Posture for Agents (Without Black-Box Scores)
9. Rate Limits That Don’t Guess: Queuing Theory for Stable, Cost-Aware Agent Systems

---

## Post 0 — Kickoff

### Toward Provable AI Governance: Why Agent Security Needs Math, Not Just Guardrails

**Description:** AI security is entering a familiar stage: early products ship with pragmatic guardrails, and then reality shows the limits of heuristics.

**Intro:**
AI security is entering a familiar stage: early products ship with pragmatic guardrails, and then reality shows the limits of heuristics. As agents become multi-step systems that act through tools and data, we need controls that are measurable, auditable, and resilient against adaptation.

This series is a set of design notes: mathematical and scientific foundations that can strengthen AI governance from first principles—information flow models, runtime verification, stability theory, and adversarial reasoning. The goal isn’t academic novelty for its own sake; it’s to move from “best-effort safety” toward properties that can be tested, monitored, and defended in enterprise reviews. Each post focuses on one foundation, what it enables, what it cannot guarantee, and how it can map to real-world telemetry and evidence.

---

## Post 1 — Game Theory

### Stop Guessing Policy Thresholds: A Game-Theoretic View of AI Guardrails

**Description:** How game-theoretic thinking can turn threshold-setting into a measurable trade-off between security loss and productivity loss.

**Intro:**
Most AI guardrails still rely on thresholds that are chosen like magic numbers: confidence cutoffs, rate limits, blocklists, and “risk scores” tuned by gut feel. That works—until you meet an adaptive adversary who learns your boundaries faster than you can tune them. In classical security, this isn’t a new problem: it’s a strategic interaction between an attacker and a defender, with costs on both sides.

Game theory gives us the vocabulary to model this explicitly—where enforcement isn’t just a rule, but a strategy, and “optimal” means balancing security loss against productivity loss under adversarial pressure. In this post, we explore what it would mean to treat policy enforcement as a two-player game, and how that lens can turn threshold-setting into a measurable, reviewable decision process rather than a permanent argument between security and engineering.

---

## Post 2 — Information Theory

### Entropy as a Security Signal: Detecting Exfiltration and Prompt Injection Without Training Data

**Description:** Why entropy/KL divergence can be a lightweight, model-agnostic early warning layer for agentic systems.

**Intro:**
When an agent is attacked, the failure mode is often visible before it is recognizable. A prompt injection attempt may not contain a known signature; data exfiltration may not match a PII pattern; a compromised workflow may still “look normal” at the surface. Information theory offers a different approach: measure properties of the behavior stream itself—uncertainty, distribution shifts, and concentration of actions—and treat unusual changes as signals worth investigating.

Shannon entropy and KL divergence are not magic detectors, but they are useful instruments: they can flag encoded payloads, repetitive context flooding, tool-call collapse, and other deviations that don’t require supervised training. This post outlines how entropy-based indicators can become a lightweight early-warning layer in agentic systems—especially when you need model-agnostic telemetry you can explain to a reviewer.

---

## Post 3 — Markov Chains

### From Tool Calls to Trajectories: Using Markov Models to Catch Agent Loops and Escalations

**Description:** Modeling agent workflows as transitions between states to flag rare/unsafe behavior patterns.

**Intro:**
A single tool call rarely tells the full story. The real risk emerges across sequences: plan → tool → plan → tool loops that never converge, sudden jumps from read-only operations into write paths, repeated denials that turn into retries, or “stuck” behaviors that inflate cost and amplify exposure. Markov chains provide a simple but powerful abstraction here: treat an agent’s workflow as transitions between states, and quantify which transitions are typical versus rare.

You don’t need a perfect behavioral model to get value—just enough structure to highlight transitions that don’t fit the baseline for that agent, that workload, or that environment. In this post we explore how trajectory-aware monitoring can complement content checks, and why behavior anomaly detection is increasingly necessary as agents become multi-step systems rather than single-turn chats.

---

## Post 4 — Catastrophe Theory

### Why AI Systems Fail Suddenly: Tipping Points, Cascades, and Preemptive Circuit Breakers

**Description:** How tipping-point thinking can improve resilience, cost control, and early-warning degradation in AI runtimes.

**Intro:**
Many AI failures don’t look dramatic at first. Latency increases slightly, retries become more frequent, tool failures start to cluster—and then the system flips from “mostly fine” to “completely unstable” in minutes. Traditional circuit breakers often fire too late because they rely on hard thresholds that only trigger after the damage is already happening.

Catastrophe theory studies exactly this kind of behavior: small continuous changes in control parameters can produce discontinuous changes in outcomes. While the math can be sophisticated, the practical takeaway is straightforward: there are detectable early warning signals before a system crosses a tipping point—rising variance, increasing autocorrelation, flickering between stable states. This post discusses how pre-failure indicators can inform smarter degradation strategies—switching models, reducing tool scope, or throttling workloads *before* a cascade begins.

---

## Post 5 — Formal Verification

### Policies You Can Prove: Temporal Logic for Auditable, Trace-Based AI Safety

**Description:** Turning prose policies into checkable properties over execution traces, producing verdicts and counterexamples.

**Intro:**
Security policies are often written as prose—“Never output PII,” “Admin actions require approval,” “Tool calls must be audited”—and then enforced through a mix of heuristics and best-effort checks. That gap between policy text and runtime reality is where audit pain lives.

Temporal logic offers a different paradigm: express policies as precise properties over sequences of events, then verify those properties against execution traces. The result isn’t a vague score; it’s a verdict with a counterexample when violated. This is how safety-critical domains build assurance, and it maps surprisingly well to agentic AI where behavior unfolds as observable traces: tool calls, approvals, data access, and outputs. In this post we explore how runtime verification can turn “guardrails” into formally checkable properties—and how that can materially improve audit readiness without turning engineering into a research project.

---

## Post 6 — Lattice-Based Access Control

### Beyond DLP Regex: Lattice-Based Information Flow Control for AI Agents

**Description:** Why content scanning fails in multi-step agent systems—and how label propagation enforces flow constraints.

**Intro:**
Most data protection in AI systems still looks like content scanning: detect sensitive patterns, redact, block, or alert. That helps—but it doesn’t fully solve the hard problem: information can be transformed. An agent can read confidential data, summarize it, paraphrase it, translate it, or split it across tool calls. Traditional DLP struggles to reason about *flows* across multi-step computation.

Lattice-based information flow control tackles this at a more fundamental level: label data and channels, propagate labels through computation, and enforce which flows are permitted. Instead of asking “Does the output contain PII?”, you ask “Is this output allowed to be derived from restricted inputs and sent to this destination?” This post introduces lattice models as a rigorous foundation for preventing cross-context leakage in agentic systems—and why this is one of the most promising directions for audit-grade data controls.

---

## Post 7 — Bayesian Inference

### Trust That Updates: Bayesian Security Posture for Agents (Without Black-Box Scores)

**Description:** Using transparent Bayesian updates to map evidence → posture changes without mysterious risk scores.

**Intro:**
Static trust models don’t match reality. In enterprise environments, the same agent can be well-behaved for weeks and then become risky due to a compromised prompt, a misconfigured tool, a new integration, or a malicious user input. The right question is not “Do we trust this agent?” but “How should our confidence update as evidence accumulates?”

Bayesian inference provides a principled way to do exactly that: start with a prior, update with observations, and map the posterior to enforcement posture—monitor, enforce, require approval, or restrict tools. The key is transparency: every trust update must be explainable in terms of observed events and their likelihood under normal versus compromised behavior. This post explores how Bayesian trust can become an accountable posture mechanism rather than a mysterious risk score—and how to keep it audit-friendly.

---

## Post 8 — Queuing Theory

### Rate Limits That Don’t Guess: Queuing Theory for Stable, Cost-Aware Agent Systems

**Description:** Deriving stability budgets from arrival/service rates to govern tokens, tool calls, and retries under load.

**Intro:**
Most rate limiting in AI systems is arbitrary: “50 requests per minute,” “10 tool calls per task,” “hard cap at X tokens.” Those limits might be conservative, but they’re rarely grounded in how the system actually behaves under load. Queuing theory offers a more rigorous way: model arrivals, service capacity, utilization, and waiting time, then set limits that keep the system stable while maximizing throughput.

This matters for agentic AI because overload isn’t just a performance problem—it becomes a security and governance problem. Retries amplify cost, tool failures create unpredictable behavior, and instability can push agents into non-converging loops. In this post we explore how stability budgets derived from queuing models can govern token spend, tool-call pressure, and API usage in a way that is measurable, explainable, and operationally defensible.

---
