---
layout: page
title: Product Roadmap
description: TealTiger roadmap for deterministic AI governance across runtime, security, cost, model, data, sustainability, and audit-grade evidence.
---

# TealTiger Product Roadmap

**Last updated:** Mar 31, 2026

TealTiger is a runtime governance SDK for agentic AI systems. Our roadmap is organized around **governance maturity**: increasing the depth of **deterministic enforcement**, **cross-dimensional correlation**, and **audit-grade evidence** across the full set of governance dimensions.

> **Roadmap note:** This page reflects current priorities and directional intent. Specific items may shift based on user feedback, integration requirements, and security considerations.

---

## Roadmap Principles (Enterprise Commitments)

### 1) Deterministic-First Enforcement
TealTiger prioritizes **deterministic outcomes** (allow/deny/require/transform/pause) over probabilistic guardrails. When signals are statistical, enforcement still applies via **deterministic thresholds and explicit policies**.

### 2) Evidence is a First-Class Product Surface
Every policy decision should be explainable and exportable as **audit-grade evidence**: what happened, which rule evaluated, why it triggered, what action was taken, and what version of policy was applied.

### 3) Dimensions are Orthogonal; Releases Increase Correlation
Governance dimensions do not “arrive” in one release and disappear in another. Releases increase:
- enforcement depth within dimensions
- correlations across dimensions
- evidence completeness and portability

### 4) Procurement-Safe by Design
We prefer stable interfaces, compatibility guarantees, and explicit non-goals over rapid churn. Enterprise adoption requires predictable behavior and traceable change.

---

## Governance Dimensions (Canonical Model)

TealTiger governs agentic AI across multiple dimensions. These dimensions are grouped into conceptual planes to keep the system coherent and scalable.

### Plane A — Runtime & Agentic Execution (What happens at run time)
These dimensions describe **behavior and control at execution time**.

- **Runtime**
  - Governance of tool calls, execution steps, runtime decisions, and enforcement points.
- **Security**
  - Prompt injection, data exfiltration, privilege misuse, policy bypass, and adversarial behavior.
- **State & Context Dimension**
  - What the agent carries *right now*: active context size, state mutation, provenance of context, and unsafe context accumulation.
- **Trajectory / Behavior Dimension**
  - Multi-step behavior: loops, escalation paths, repeated sequences, and tool-transition patterns.
- **Intent Drift Dimension**
  - Goal integrity: changes in declared objectives, constraint removal attempts, scope drift, and divergence from original intent.
- **Authority & Capability Dimension**
  - What the agent is *allowed* to do: tool permissions, scope constraints, privilege ceilings, and transitive capability gain.
- **Temporal Dimension**
  - Time-based controls: TTLs, session duration, stale context prevention, policy versioning at decision time.
- **Signal & Anomaly Dimension**
  - Runtime signals: entropy, distribution shift, parameter variance, and deviation from known baselines—enforced via deterministic thresholds.
- **Memory (Reframed Properly)**
  - Persistence governance: memory read/write policies, TTL/expiry, memory poisoning defenses, provenance, and cross-session leakage prevention.
- **Human & External Influence Dimension**
  - Influence tracking: user overrides, tool-injected instructions, retriever sources, trust tiers, and “untrusted influence” controls.

### Plane B — Model & Data Governance (What the system reasons over)
These dimensions govern **model choice and data provenance**, not just runtime outputs.

- **Model**
  - Model selection, version drift, fallback models, capability deltas, and model policy constraints.
- **Data**
  - Retrieval sources, provenance, classification, handling constraints, and data boundary enforcement.

### Plane C — Organizational & Economic Governance (What keeps AI safe and viable)
These dimensions connect runtime governance to **business constraints and accountability**.

- **Cost**
  - Token/compute budgets, runaway agent spending controls, cost ceilings, and policy-gated spend.
- **Sustainability**
  - Energy-aware governance, usage externalities, long-horizon efficiency constraints, and sustainability reporting artifacts.
- **Compliance**
  - Control mappings (e.g., internal policies, regulatory expectations), approvals, and evidence traceability.
- **Risk-Assurance**
  - Risk acceptance workflows, control coverage assertions, sign-off records, and residual risk documentation.

### Plane D — Evidence & Knowledge System (How trust is proven)
These dimensions ensure governance is **auditable, reviewable, and explainable**.

- **Evidence**
  - Audit-grade records: decisions, reasons, rule evaluations, enforcement actions, hashes, and export formats.
- **Frameworks**
  - Mappings to governance frameworks (NIST, ISO, EU AI Act, internal controls) as structured evidence overlays.
- **Foundations**
  - First principles that motivate deterministic governance; canonical reasoning references.
- **Insights**
  - Cross-cutting engineering guidance and design notes; not enforcement-critical but essential for adoption.

---

## Release Themes (Governance Maturity)

### v1.1.0 — Deterministic Foundations for Security & Cost (Current / Established)
**Theme:** Prove deterministic governance works in production with high-signal controls.

**Primary outcomes**
- Deterministic enforcement for **security** and **cost** controls
- Evidence records for policy decisions at runtime

**Representative capabilities**
- Runtime security policy enforcement (allow/deny/pause) for high-risk behaviors
- Cost ceilings and runaway execution controls
- Evidence export for decisions and violations

**Enterprise value**
- Establishes governance credibility: “policy is enforceable and auditable,” not advisory.

---

### v1.2.x — Cross-Dimensional Correlation (Near-Term)
**Theme:** Policies that reason across dimensions—security × runtime × model × cost—backed by consistent evidence.

**Key outcomes**
- Correlate runtime behavior with model context and spend constraints
- Expand evidence into an end-to-end decision narrative

**Planned capability areas**
- **Trajectory / Behavior Dimension**
  - Loop detection, escalation path constraints, step caps, and deterministic loop breakers
- **Authority & Capability Dimension**
  - Tool capability classification, scope constraints, transitive capability restrictions
- **Temporal Dimension**
  - TTLs for sessions and context, stale-context prevention, time-based policy constraints
- **Model Dimension (initial)**
  - Model version drift awareness in policies (e.g., require approval on change)
- **Evidence (strengthening)**
  - Better “why” narratives: rule evaluation trace, policy version pinning, export-ready evidence bundles

**Enterprise value**
- Moves beyond single-dimension controls to govern the **intersections where failures occur**.

---

### v1.3.x — Multi-Plane Governance (Enterprise-Grade)
**Theme:** System-level governance across runtime, model, data, cost, sustainability, compliance, and risk assurance—with audit-grade evidence.

**Key outcomes**
- Extend governance beyond runtime events into organizational assurance
- Make compliance and sustainability enforceable dimensions—not just reports

**Planned capability areas**
- **Memory (Reframed Properly)**
  - Memory read/write policies, provenance requirements, poisoning resistance patterns, TTL/expiry enforcement
- **Human & External Influence Dimension**
  - Trust tiers for sources, untrusted influence gating, tool-injected instruction controls
- **State & Context Dimension**
  - Context mutation tracking, provenance tagging, deterministic context bounds, forbidden context fields
- **Data Dimension (strengthening)**
  - Retrieval source allowlists, provenance enforcement, data boundary constraints
- **Cost + Sustainability (joint governance)**
  - Policy-gated spend, energy-aware constraints, cost/sustainability caps by environment or workload
- **Compliance + Risk-Assurance**
  - Approval workflows, risk acceptance artifacts, control coverage evidence, exportable audit packs
- **Framework overlays**
  - Structured mappings of TealTiger evidence to governance frameworks (internal and external)

**Enterprise value**
- Enables organizations to govern AI systems as **systems**—not as single prompts or isolated model calls.

---

## What We Mean by “Done” (Definition of Governance Capability)

A governance capability is considered mature when it supports:
1) **Deterministic enforcement** (allow/deny/require/transform/pause)  
2) **Deterministic evidence** (who/what/when/why with versioned policies)  
3) **Operational integration** (export paths to logging/monitoring and audit workflows)

---

## Non-Goals (For Clarity)

To remain procurement-safe and technically defensible, TealTiger explicitly avoids:
- “Magic” risk scoring without explainability
- Claims of universal safety guarantees
- Training-time model alignment as the primary mechanism
- Unbounded policy complexity that cannot be audited or reproduced

---

## Compatibility & Stability

- We aim to keep policy definitions and evidence formats stable across minor versions whenever possible.
- Breaking changes will be versioned explicitly with migration notes and clear upgrade paths.
- Evidence schemas will prioritize backward-compatibility and explicit versioning.

---

## How to Engage (Enterprise-Friendly)

- **Feature requests:** Submit issues with a clear use case, threat model, and desired enforcement outcome.
- **Design feedback:** We prioritize feedback tied to deterministic enforcement and evidence requirements.
- **Adoption support:** For enterprise pilots, we can provide reference policies and evidence export guidance.

---

## Quick Index of Dimensions (for navigation)

Runtime · Security · State & Context · Trajectory/Behavior · Intent Drift · Authority/Capability · Temporal · Signal/Anomaly · Memory · Human/External Influence · Compliance · Cost · Data · Evidence · Foundations · Frameworks · Insights · Model · Risk-Assurance · Sustainability
``
