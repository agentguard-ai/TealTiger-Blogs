---
title: "Governance Frameworks and TealTiger"
description: "How TealTiger operationalizes governance frameworks through deterministic enforcement — turning standards into executable controls."
layout: page
permalink: /governance/frameworks/frameworks-governance/
tags:
  - frameworks
  - governance
  - nist
  - iso
  - eu-ai-act
  - owasp
breadcrumbs:
  - title: Home
    url: /
  - title: Governance
    url: /governance/
  - title: Frameworks
    url: /governance/frameworks/
  - title: Frameworks Governance
    url: /governance/frameworks/frameworks-governance/
---

# Governance Frameworks and TealTiger

Governance frameworks provide **structure and shared language**, but they do not enforce behavior by themselves. TealTiger is designed to **operationalize governance frameworks through deterministic enforcement**.

---

## The Problem with Framework-Only Governance

<div class="tt-flow">
  <div class="tt-flow__step">Framework Requirement</div>
  <span class="tt-flow__arrow">→</span>
  <div class="tt-flow__step">Interpret</div>
  <span class="tt-flow__arrow">→</span>
  <div class="tt-flow__step">Write Policy Document</div>
  <span class="tt-flow__arrow">→</span>
  <div class="tt-flow__step">Manual Implementation</div>
  <span class="tt-flow__arrow">→</span>
  <div class="tt-flow__step">Periodic Audit</div>
  <span class="tt-flow__arrow">→</span>
  <div class="tt-flow__step tt-flow__step--warn">Gap Found ↻</div>
</div>

Most frameworks:
- Describe *what* good governance looks like
- Do not specify *how* controls are enforced
- Rely on manual interpretation and process maturity

This creates gaps between intent and execution — gaps that widen in agentic systems where behavior changes at runtime.

---

## How TealTiger Aligns with Frameworks

TealTiger maps governance frameworks to **explicit, enforceable controls**:

<div class="tt-flow">
  <div class="tt-flow__step">Framework Requirement</div>
  <span class="tt-flow__arrow">→</span>
  <div class="tt-flow__step tt-flow__step--accent">TealTiger Contract</div>
  <span class="tt-flow__arrow">→</span>
  <div class="tt-flow__step tt-flow__step--accent">Runtime Enforcement</div>
  <span class="tt-flow__arrow">→</span>
  <div class="tt-flow__step tt-flow__step--accent">Decision Evidence</div>
  <span class="tt-flow__arrow">→</span>
  <div class="tt-flow__step tt-flow__step--accent">Audit-Ready Proof</div>
</div>

| Framework | TealTiger Mapping |
|-----------|------------------|
| **NIST AI RMF** | Govern → contracts, Map → risk classification, Measure → evidence, Manage → enforcement |
| **ISO/IEC 42001** | Control objectives → enforceable policies with versioned evidence |
| **EU AI Act** | Technical control evidence for high-risk AI system requirements |
| **OWASP Agentic Top 10** | Runtime security controls mapped to agentic threat categories |

---

## Contract-to-Framework Mapping

TealTiger governance contracts serve as the bridge between abstract framework requirements and operational enforcement:

- **Define control intent** — what the framework requires, expressed as policy
- **Enforce control behavior** — deterministic evaluation at runtime decision points
- **Produce immutable evidence** — decision-grade artifacts tied to policy versions

This allows organizations to *prove* framework alignment, not just claim it.

---

## Important Note

TealTiger does not certify compliance with any framework. It provides the technical controls and evidence that support an organization's compliance program. Framework alignment is a continuous process that requires organizational commitment beyond tooling.

---

## Practical Checklist

- [ ] Identify target framework requirements relevant to your AI systems
- [ ] Map requirements to TealTiger governance contracts
- [ ] Configure enforcement policies that satisfy control objectives
- [ ] Export decision evidence aligned to framework audit expectations
- [ ] Review mappings when frameworks are updated

---

## Related

- [Compliance Enablement](/governance/compliance/) — Compliance-enabling controls
- [Evidence & Audit](/governance/evidence/) — Decision-grade evidence
- [Governance Foundations](/governance/foundations/) — Core principles
