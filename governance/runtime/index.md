---
layout: page
title: Runtime Governance
---

Runtime Governance explains how TealTiger governs **agent behavior while systems are actively executing**.

In agentic AI systems, the highest risk is not design‑time intent — it is **runtime behavior**. Once deployed, agents can make decisions, invoke tools, access data, and interact with systems in ways that exceed their original scope if not explicitly controlled.

TealTiger treats runtime as the **primary enforcement layer** for governance.

## What This Hub Covers

- Enforcing governance policies during agent execution
- Governing prompt execution and tool invocation
- Preventing scope expansion and unauthorized actions
- Detecting and stopping policy violations in real time
- Producing deterministic runtime enforcement decisions

## Why Runtime Governance Matters

Without runtime governance:
- Policies exist only on paper
- Agents can exceed intended authority
- Violations are detected after impact
- Observability replaces enforcement

TealTiger enforces governance **at the moment decisions are made**, ensuring that every agent action is evaluated against approved contracts and policies.

## Core Articles

- /governance/runtime/runtime-governance/
  - How TealTiger enforces deterministic governance controls during live agent execution.
