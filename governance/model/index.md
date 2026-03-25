---
layout: page
title: Model Governance
---

Model Governance explains how TealTiger governs **which AI models are allowed**, **for what purposes**, and **under what risk boundaries** in agentic AI systems.

The Model Governance Hub defines **normative controls** for model approval, task binding, risk boundary enforcement, and lifecycle accountability.

In agentic environments, uncontrolled model usage introduces serious risks — including inconsistent behavior, compliance exposure, cost volatility, and untraceable decision paths.

TealTiger treats model selection and usage as a **governance decision**, not a runtime convenience.

## What This Hub Covers

- Governing approved models and providers
- Binding models to task types and risk classifications
- Preventing unauthorized or implicit model switching
- Enforcing deterministic model usage policies
- Producing audit‑ready evidence of model decisions

## Why Model Governance Matters

Without explicit model governance:
- Different models may be used silently for the same task
- Risk characteristics change without review
- Compliance assumptions break
- Cost and behavior become unpredictable

TealTiger enforces **explicit, contract‑defined model usage**, ensuring consistency, traceability, and control.

---
layout: page
title: Model Governance
---

Model Governance defines how TealTiger controls **model capability, selection, and drift** in agentic AI systems.

In agentic systems, model choice is not a static configuration. Models may be selected, escalated, or switched dynamically at runtime. This hub focuses on **governing model capability as a first‑class risk surface**, ensuring that only approved, risk‑appropriate models are allowed to execute.

## What This Hub Covers

- Approved model registries for enterprise AI
- Task–model binding to prevent silent capability escalation
- Detection and prevention of shadow model usage
- Runtime enforcement of model approval and escalation
- Audit‑grade evidence of model governance decisions

## Why Model Governance Matters

Without explicit model governance:

- Agents silently escalate capability
- Unapproved or experimental models execute in production
- Risk posture changes without review
- Cost and compliance exposure increases
- Audits cannot explain why a model was allowed

TealTiger treats model choice as a **governed execution decision**, not an implementation detail.

## Core Articles

- [Model Governance Overview](/governance/model/model-governance/)
  - The primary model for controlling AI model capability, approval, and enforcement in agentic systems.

## Additional Articles

- [Approved Model Registries for Enterprise AI](/governance/model/approved-model-registries/)
- [Task–Model Binding: Preventing Capability Drift](/governance/model/task-model-binding/)
- [Shadow Models and Hidden Risk in Agentic Systems](/governance/model/shadow-models-hidden-risk/)

These articles expand the model governance framework by showing how **approved registries**, **capability binding**, and **runtime enforcement** prevent unintentional escalation and hidden model risk.
