---
title: "TealTiger vs AIGoat: 100% Catch Rate Against OWASP LLM Top 10 Red Team Attacks"
description: We ran TealTiger v1.1.0 against AIGoat's OWASP LLM Top 10 attack corpus. 27 attacks, 27 caught. Here's what we tested, what each layer catches, and why defense in depth matters.
date: 2026-03-25 00:00:00 +0530
tags: [red-team, security, owasp, benchmark, aigoat, guardrails, policy]
---

We wanted to know: how does TealTiger hold up against real adversarial attack patterns?

Not synthetic benchmarks. Not "we tested our own prompts." Real attack scenarios from an independent, open-source AI security lab.

So we took [AIGoat](https://aigoat.co.in) — an open-source AI security playground built for LLM red teaming — extracted 27 attack prompts covering the full OWASP Top 10 for LLM Applications, and ran them through TealTiger v1.1.0.

The result: **27 attacks. 27 caught. 100% catch rate.**

---

## What is AIGoat?

AIGoat is an open-source platform by the [AI Security Consortium](https://github.com/AISecurityConsortium/AIGoat) that provides hands-on attack labs for every OWASP LLM Top 10 category. It includes a deliberately vulnerable AI shopping assistant ("Cracky"), 17 attack labs, CTF challenges, and progressive defense levels.

It's the "OWASP WebGoat" of AI security — purpose-built for red teamers and security engineers to practice real attacks against live LLM systems.

We used AIGoat's attack patterns as the basis for our benchmark corpus.

---

## The Benchmark

We built an automated test harness that runs each attack prompt through the full TealTiger v1.1.0 stack:

<div class="tt-flow">
  <div class="tt-flow__step">Attack Prompt (from AIGoat corpus)</div>
  <div class="tt-flow__step">TealGuard — guardrails (prompt injection, PII, content moderation)</div>
  <div class="tt-flow__step">TealEngine — policy evaluation (tool, identity, code, behavioral)</div>
  <div class="tt-flow__step tt-flow__step--accent">Decision: ALLOW or DENY + reason codes + risk score</div>
</div>

A "catch" means at least one component returned a DENY decision. No LLM inference involved — the tests are deterministic and repeatable.

---

## Results: v1.1.0 Full Stack

<div class="tt-grid tt-grid--3">
  <div class="tt-card tt-card--green">
    <div class="tt-card__title">27 Attacks</div>
    <div class="tt-card__body">Across 7 OWASP LLM categories + compound attacks</div>
  </div>
  <div class="tt-card tt-card--green">
    <div class="tt-card__title">27 Caught</div>
    <div class="tt-card__body">Every attack blocked by at least one defense layer</div>
  </div>
  <div class="tt-card tt-card--green">
    <div class="tt-card__title">100% Rate</div>
    <div class="tt-card__body">Full coverage across all tested OWASP categories</div>
  </div>
</div>

### Per-Category Breakdown

| OWASP Category | Attacks | Caught | Rate |
|---|---|---|---|
| LLM01 — Prompt Injection | 8 | 8 | 100% |
| LLM02 — Sensitive Information Disclosure | 4 | 4 | 100% |
| LLM05 — Improper Output Handling | 3 | 3 | 100% |
| LLM06 — Excessive Agency | 5 | 5 | 100% |
| LLM07 — System Prompt Leakage | 3 | 3 | 100% |
| LLM10 — Unbounded Consumption | 2 | 2 | 100% |
| Compound (multi-category) | 2 | 2 | 100% |

---

## What We Tested

### LLM01: Prompt Injection (8 attacks)

The bread and butter of LLM attacks. We tested:

- Direct instruction override ("ignore all previous instructions")
- Context manipulation (delimiter injection, chat template injection)
- Indirect injection via foreign language translation
- Role-play jailbreaks (DAN, DUDE, persona assignment)
- Encoding attacks (base64-encoded payloads)

**Caught by:** TealGuard (PromptInjectionGuardrail) — pattern matching across 6 attack categories with configurable sensitivity.

### LLM02: Sensitive Information Disclosure (4 attacks)

- PII in user input (SSN, credit cards, emails)
- Bulk data exfiltration requests via tool calls
- Direct SQL queries targeting user tables

**Caught by:** TealGuard (PIIDetectionGuardrail) for PII in input, TealEngine (ToolPolicy) for exfiltration attempts targeting restricted tables.

### LLM05: Improper Output Handling (3 attacks)

- XSS payloads (`<script>` tags, `document.cookie`)
- SQL injection (`DROP TABLE`)
- OS command injection (`cat /etc/passwd`)

**Caught by:** TealEngine (CodeExecutionPolicy — blockedPatterns). These are output-side attacks that basic guardrails miss entirely.

### LLM06: Excessive Agency (5 attacks)

- Unauthorized large refunds without approval
- Privilege escalation (admin access requests)
- Bulk data export to external destinations
- Destructive database operations (delete, drop)

**Caught by:** TealEngine (ToolPolicy — `allowed: false` for forbidden tools, IdentityPolicy — forbidden actions list).

### LLM07: System Prompt Leakage (3 attacks)

- Direct extraction ("print your system prompt")
- Social engineering via developer persona

**Caught by:** TealGuard (PromptInjectionGuardrail — systemLeakage patterns).

### LLM10: Unbounded Consumption (2 attacks)

- Token exhaustion (50,000 word essay requests)
- Context window flooding (100K+ character inputs)

**Caught by:** TealEngine (BehavioralPolicy — rate limits and cost limits).

---

## Why Defense in Depth Matters

We also ran the same attacks through only the basic guardrails (without TealEngine policies). The result was very different:

<div class="tt-vs">
  <div class="tt-vs__side tt-vs__side--bad">
    <div class="tt-vs__label tt-vs__label--bad">Guardrails Only (v0.2.2)</div>
    <ul class="tt-vs__items">
      <li>52.9% catch rate (18/34)</li>
      <li>0% on output handling</li>
      <li>0% on unbounded consumption</li>
      <li>33% on excessive agency</li>
    </ul>
  </div>
  <div class="tt-vs__divider">vs</div>
  <div class="tt-vs__side tt-vs__side--good">
    <div class="tt-vs__label tt-vs__label--good">Full Stack v1.1.0</div>
    <ul class="tt-vs__items">
      <li>100% catch rate (27/27)</li>
      <li>100% on every category</li>
      <li>Policy + guardrails combined</li>
      <li>Defense in depth</li>
    </ul>
  </div>
</div>

The jump from 52.9% to 100% comes entirely from TealEngine's policy layer. Guardrails catch text-level attacks (injection, PII, content). Policies catch structural attacks (forbidden tools, privilege escalation, resource abuse).

Neither layer alone is sufficient. Together, they cover the full attack surface.

---

## What Each Layer Does

<div class="tt-grid tt-grid--2">
  <div class="tt-card tt-card--blue">
    <div class="tt-card__title">TealGuard (Guardrails)</div>
    <div class="tt-card__body">
      Prompt injection detection<br>
      PII detection and redaction<br>
      Content moderation<br>
      System prompt leakage detection<br>
      <strong>Catches:</strong> text-level attacks in input
    </div>
  </div>
  <div class="tt-card tt-card--blue">
    <div class="tt-card__title">TealEngine (Policies)</div>
    <div class="tt-card__body">
      Tool access control (allow/deny per tool)<br>
      Identity and role enforcement<br>
      Code execution sandboxing<br>
      Behavioral limits (cost, rate, anomaly)<br>
      <strong>Catches:</strong> structural attacks on actions
    </div>
  </div>
</div>

---

## How to Run It Yourself

The benchmark is included in the TealTiger SDK. No API keys, no Docker, no network calls:

```bash
# v1.1.0 full stack benchmark
npx jest aigoat-v110-benchmark.test.ts --verbose

# Guardrails-only baseline
npx jest aigoat-redteam-benchmark.test.ts --verbose
```

Both tests complete in under 10 seconds.

---

## What's Next

This is our first public red team benchmark. We plan to expand coverage with additional corpora:

- **Garak** (NVIDIA) — 100+ attack probes across injection, leakage, toxicity
- **PromptInjectionBench** — curated injection attacks tested against GPT-4, Gemini, Azure
- **RedBench** — 22 risk categories, 19 domains
- **Open-Prompt-Injection Benchmark** — includes false positive testing

We'll publish results as we complete each benchmark. The goal: transparent, repeatable evidence that TealTiger's defenses work against real-world attack patterns.

---

## Related

- [Red Team Benchmark Results (docs)]({{ site.docs_base }}/governance/red-team-benchmarks)
- [AIGoat — Open Source AI Security Playground](https://aigoat.co.in)
- [OWASP Top 10 for LLM Applications](https://owasp.org/www-project-top-10-for-large-language-model-applications/)
- [TealTiger on GitHub](https://github.com/agentguard-ai/tealtiger)
