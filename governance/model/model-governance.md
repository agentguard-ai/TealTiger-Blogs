---
title: "Model Governance and Control"
description: "Explicit governance over which AI models are allowed, for which tasks, and under what risk boundaries."
layout: page
permalink: /governance/model/model-governance/
tags:
  - model-governance
  - agentic-ai
  - governance
  - deterministic-enforcement
breadcrumbs:
  - title: Home
    url: /
  - title: Governance
    url: /governance/
  - title: Model
    url: /governance/model/
  - title: Model Governance
    url: /governance/model/model-governance/
---

# Model Governance and Control

Not all models are equal. Different models carry different risk profiles, cost characteristics, and compliance implications. In agentic systems, uncontrolled model switching is a governance failure.

TealTiger introduces **explicit model governance** — deterministic control over which models run, for what purposes, and under what constraints.

---

## Why Model Governance Matters

<div class="tt-vs">
  <div class="tt-vs__side tt-vs__side--bad">
    <div class="tt-vs__label tt-vs__label--bad">Ungoverned</div>
    <ul class="tt-vs__items">
      <li>Runtime picks cheapest/fastest</li>
      <li>Inconsistent behavior</li>
      <li>Compliance exposure</li>
      <li>Cost volatility</li>
    </ul>
  </div>
  <div class="tt-vs__divider">vs</div>
  <div class="tt-vs__side tt-vs__side--good">
    <div class="tt-vs__label tt-vs__label--good">Governed</div>
    <ul class="tt-vs__items">
      <li>Policy evaluates task + risk + environment</li>
      <li>Approved model for task type</li>
      <li>Consistent decisions</li>
      <li>Auditable model usage</li>
    </ul>
  </div>
</div>

Without model governance:
- Different models may be used silently for the same task
- Risk characteristics change without review
- Compliance assumptions break when models are swapped
- Cost becomes unpredictable

---

## What Is Governed

| Dimension | Control |
|-----------|---------|
| **Model allowlist** | Only named models/versions are permitted |
| **Task binding** | Models are approved for specific task categories |
| **Risk classification** | High-risk tasks require approved high-capability models |
| **Environment scoping** | Different models for dev vs staging vs production |
| **Cost tier** | Premium models require explicit budget approval |

---

## Deterministic Model Selection

Model usage is driven by governance contracts, not runtime heuristics or framework defaults.

<div class="tt-flow">
  <div class="tt-flow__step">Task Request</div>
  <span class="tt-flow__arrow">→</span>
  <div class="tt-flow__step">Policy: Task Type?</div>
  <span class="tt-flow__arrow">→</span>
  <div class="tt-flow__step">Policy: Environment?</div>
  <span class="tt-flow__arrow">→</span>
  <div class="tt-flow__step">Policy: Risk Level?</div>
  <span class="tt-flow__arrow">→</span>
  <div class="tt-flow__step">Model Approved?</div>
</div>
<div class="tt-flow" style="margin-top: 8px;">
  <div class="tt-flow__step tt-flow__step--accent">Yes → Execute with approved model</div>
  <span class="tt-flow__arrow" style="opacity: 0.5;">·</span>
  <div class="tt-flow__step tt-flow__step--warn">No → DENY: MODEL_NOT_APPROVED</div>
</div>

Every model decision is:
- **Explicit** — no implicit fallbacks
- **Traceable** — policy version and model version recorded
- **Deterministic** — same task + same context = same model selection

---

## Preventing Silent Model Drift

Model drift happens when:
- A framework auto-selects a different model version
- A developer changes a default without review
- A provider deprecates a model and the system falls back silently

TealTiger prevents this by treating model selection as a **policy-enforced decision**, not a configuration default. Changes require policy updates, which are versioned and reviewable.

---

## Practical Checklist

- [ ] Maintain an allowlist of approved models per environment
- [ ] Bind models to task categories (e.g., "summarization" → approved models only)
- [ ] Require explicit approval for premium/expensive model tiers
- [ ] Log model selection decisions with policy version
- [ ] Alert on model usage outside approved boundaries
- [ ] Review model governance quarterly as providers update offerings

---

## Related

- [Cost Governance](/governance/cost/) — Model tier constraints and budget enforcement
- [Risk Assurance](/governance/risk-assurance/) — Risk classification for model selection
- [Governance Foundations](/governance/foundations/) — Contract-first principles
