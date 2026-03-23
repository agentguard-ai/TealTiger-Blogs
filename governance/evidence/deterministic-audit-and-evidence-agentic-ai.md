---
layout: page
title: "Deterministic Audit & Evidence for Agentic AI"
description: "How TealTiger produces decision-grade evidence for agentic systems — replayable, versioned, and audit-ready by construction."
permalink: /governance/evidence/deterministic-audit-and-evidence-agentic-ai/
tags:
  - audit-evidence
  - governance
  - agentic-ai
  - deterministic-enforcement
  - policy-versioning
breadcrumbs:
  - title: Home
    url: /
  - title: Governance
    url: /governance/
  - title: Evidence
    url: /governance/evidence/
  - title: Audit & Evidence
    url: /governance/evidence/deterministic-audit-and-evidence-agentic-ai/
---


Audits fail when systems rely on logs to explain decisions after the fact. In agentic systems, governance requires **evidence generated at decision time**, not forensic reconstruction later.

TealTiger treats audit and evidence as **first-class runtime outputs**, tightly coupled to policy evaluation and enforcement.

---

## Why Logs Are Not Evidence

<div class="tt-vs">
  <div class="tt-vs__side tt-vs__side--bad">
    <div class="tt-vs__label tt-vs__label--bad">Traditional Logging</div>
    <ul class="tt-vs__items">
      <li>Action occurs</li>
      <li>Log entry written</li>
      <li>Analyst reviews later</li>
      <li>Reconstruct "why"</li>
    </ul>
  </div>
  <div class="tt-vs__divider">vs</div>
  <div class="tt-vs__side tt-vs__side--good">
    <div class="tt-vs__label tt-vs__label--good">Decision-Grade Evidence</div>
    <ul class="tt-vs__items">
      <li>Policy evaluated</li>
      <li>Decision made</li>
      <li>Evidence emitted</li>
      <li>Replay and verify</li>
    </ul>
  </div>
</div>

Traditional observability answers *what happened*. Governance must answer *why it was allowed*.

Common gaps in log-based approaches:
- Logs lack policy context (which rule matched?)
- Decisions cannot be replayed (what would happen with the same inputs?)
- Policy versions are unclear (which policy was in effect?)
- Exceptions are undocumented (who approved the override?)

In agentic systems, these gaps compound quickly across multi-step executions.

---

## Evidence as a Governance Artifact

TealTiger produces structured evidence **per decision**:

<div class="tt-flow">
  <div class="tt-flow__step">Agent Action</div>
  <span class="tt-flow__arrow">→</span>
  <div class="tt-flow__step">Decision Point</div>
  <span class="tt-flow__arrow">→</span>
  <div class="tt-flow__step">Policy Evaluation</div>
  <span class="tt-flow__arrow">→</span>
  <div class="tt-flow__step">Decision: ALLOW / DENY / MODIFY / APPROVE / HALT</div>
  <span class="tt-flow__arrow">→</span>
  <div class="tt-flow__step tt-flow__step--accent">Evidence Record</div>
</div>
<div class="tt-grid" style="margin-top: 12px;">
  <div class="tt-card tt-card--accent">
    <div class="tt-card__icon">📋</div>
    <div class="tt-card__title">Decision outcome</div>
    <div class="tt-card__desc">What was decided</div>
  </div>
  <div class="tt-card tt-card--accent">
    <div class="tt-card__icon">📜</div>
    <div class="tt-card__title">Policy ID + version</div>
    <div class="tt-card__desc">Which policy applied</div>
  </div>
  <div class="tt-card tt-card--accent">
    <div class="tt-card__icon">🎯</div>
    <div class="tt-card__title">Matched rule</div>
    <div class="tt-card__desc">Which rule triggered</div>
  </div>
  <div class="tt-card tt-card--accent">
    <div class="tt-card__icon">📸</div>
    <div class="tt-card__title">Context snapshot</div>
    <div class="tt-card__desc">Minimally sufficient context</div>
  </div>
  <div class="tt-card tt-card--accent">
    <div class="tt-card__icon">🔗</div>
    <div class="tt-card__title">Correlation ID</div>
    <div class="tt-card__desc">Cross-system traceability</div>
  </div>
  <div class="tt-card tt-card--accent">
    <div class="tt-card__icon">🕐</div>
    <div class="tt-card__title">Timestamp</div>
    <div class="tt-card__desc">When the decision occurred</div>
  </div>
</div>

Every evidence record answers:
- **What** decision was made
- **Why** (which policy, which rule)
- **When** (timestamp, execution step)
- **With what context** (minimally sufficient, sensitivity-aware)
- **Under which policy version**

---

## Replayability and Assurance

Replayability is the key property that separates evidence from logs:

> Given the same policy version and context, the same decision would occur.

This enables:
- **Internal assurance reviews** — verify decisions without re-running agents
- **External audits** — demonstrate control effectiveness with proof
- **Incident postmortems** — understand exactly why something was allowed or denied
- **Regression testing** — ensure policy changes produce expected outcomes

---

## Policy Versioning and Traceability

Every decision is tied to a **specific policy version**. This prevents silent policy drift.

| Property | What it ensures |
|----------|----------------|
| **Versioned policies** | Behavior is tied to the exact policy in effect |
| **Change traceability** | Policy updates are explicit and reviewable |
| **Time-bound exceptions** | Overrides expire and are recorded |
| **Decision replay** | Any past decision can be verified against its policy version |

---

## Practical Audit Patterns

### Decision Timelines
Reconstruct the full sequence of governance decisions for any agent run — what was allowed, denied, modified, or escalated at each step.

### Exception Registers
Track every approval override: who approved, under what policy, with what context, and when the exception expires.

### Evidence Export
Send decision artifacts to your audit store, SIEM, or compliance platform:
- JSONL for structured ingestion
- HTTP sink for real-time streaming
- Correlation IDs for cross-system tracing

---

## Practical Checklist

- [ ] Emit structured evidence for every governance decision
- [ ] Include policy version in every evidence record
- [ ] Capture minimally sufficient context (avoid logging sensitive data)
- [ ] Support evidence replay for audit and assurance
- [ ] Export evidence to your audit/SIEM infrastructure
- [ ] Maintain exception registers with time-bound approvals

---

## Related

- [Governance Foundations](/governance/foundations/) — Contract-first principles
- [Runtime Governance](/governance/runtime/) — Where decisions happen
- [Compliance Enablement](/governance/compliance/) — Using evidence for compliance
