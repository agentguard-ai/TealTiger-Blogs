---
layout: page
title: "Risk & Assurance"
permalink: /governance/risk/
show_docs_cta: false
breadcrumbs:
  - title: Home
    url: /
  - title: Governance
    url: /governance/
  - title: Risk & Assurance
    url: /governance/risk/
---

_Last updated: 2026-03-03_

Risk & Assurance is the governance domain that answers two questions:

1. **What can go wrong, how bad can it get, and how likely is it?** (risk)
2. **What evidence proves the system stayed within acceptable bounds?** (assurance)

This is not just “security risk.” It includes **agent autonomy risk**, **economic risk**, **compliance and accountability risk**, and **operational risk** across the full AI lifecycle.

---

## What “risk” means in AI governance

AI risk is the combination of:

- **Hazards**: failure modes (prompt injection, tool misuse, data leakage, runaway spend, unsafe actions)
- **Exposure**: where the system connects to the world (tools, APIs, data, networks, users, third parties)
- **Impact**: what damage looks like (financial loss, compliance breach, integrity loss, safety incident)
- **Likelihood**: how often conditions occur (threat frequency + control effectiveness)
- **Residual risk**: what remains after controls are applied

A practical stance: **Risk is managed by boundaries**—what the system is allowed to do, under which conditions, with what limits, and with what required evidence.

---

## What “assurance” means (and why logs are not enough)

Assurance is the ability to **demonstrate**—to reviewers, auditors, and stakeholders—that governance controls operated as intended.

Assurance requires:

- **Deterministic enforcement** (controls that actually block/allow at runtime)
- **Policy traceability** (what rule applied, why it applied)
- **Evidence artifacts** (structured events, decisions, proofs, approvals)
- **Reproducibility** (can you replay the “why” of an outcome later?)

Logs help, but assurance needs **governance-grade evidence**: complete, consistent, tamper-resistant, and queryable by policy intent.

If you want the deep argument:  
→ {% post_url 2026-02-27-logging-is-not-governance %}

---

## Common AI risk categories (practical)

### 1) Agent autonomy & tool-use risk
- Agents can **take actions**, not just generate text.
- Tool access expands blast radius: file systems, emails, payments, infra, ticketing, data stores.

Governance response:
- Enforce **tool allowlists**, **scopes**, **rate limits**, and **approval gates** at runtime.
- Require **explicit intent** and **justification** for high-impact actions.

### 2) Integrity risk (wrong actions, wrong outcomes)
- Agents can perform plausible but incorrect steps.
- “Looks right” is not enough—especially with automated execution.

Governance response:
- Add **verification steps**, **checkpoints**, and **human-in-the-loop** gates for high-risk operations.
- Enforce **idempotency**, **dry-run**, and **safe execution** modes where possible.

### 3) Data risk (confidentiality, privacy, and leakage)
- Tool calls and retrieval can exfiltrate sensitive data.
- Prompt injection can redirect tools to fetch/emit secrets.

Governance response:
- Apply **data classification boundaries** and **redaction rules** before output and tool calls.
- Record evidence: what data was accessed, under what policy, and why.

### 4) Economic risk (denial-of-wallet, runaway spend)
- Autonomous loops can create unexpected cost escalation.
- “Budget” becomes a security boundary when spend is a path to disruption.

Governance response:
- Enforce **budgets**, **per-action cost ceilings**, and **kill-switches**.
- Measure and control cost at runtime, not after the bill arrives.

Foundational view:  
→ {% post_url 2026-02-27-cost-is-a-security-boundary %}

### 5) Compliance & accountability risk
- Who approved what? Who is responsible when an agent acts?
- Regulators and internal governance will demand clear accountability.

Governance response:
- Use **approval workflows**, **role-based authority**, and **decision provenance**.
- Ensure policies map to responsible owners and required evidence.

---

## Risk management as governance controls (a simple playbook)

### Step 1 — Define boundaries
- What tools are allowed?
- What data can be accessed?
- What actions require approval?
- What budgets/limits apply?

### Step 2 — Enforce boundaries at runtime
- Block disallowed tool calls and unsafe actions
- Gate high-risk operations via approvals
- Apply budgets and rate limits
- Fail closed when policy cannot be evaluated

Runtime perspective:  
→ {% post_url 2026-02-27-why-runtime-enforcement-beats-static-evaluation %}

### Step 3 — Generate assurance evidence
Capture governance-grade evidence for every significant decision:
- policy version + rule ID
- evaluation result + reasons
- actions attempted/allowed/blocked
- approvals + approver identity (if applicable)
- resource usage and cost
- incident flags and kill-switch activation

### Step 4 — Review, test, and improve
- Run “golden corpus” scenarios regularly
- Test injection resistance and tool misuse paths
- Validate cost controls with worst-case simulations
- Track residual risk and update policies

---

## How this connects to other governance hubs

Risk & Assurance is the connective tissue between the hubs:

- **Runtime Governance** defines enforcement points and action boundaries  
  /governance/runtime/

- **Cost Governance** constrains economic blast radius  
  /governance/cost/

- **Audit & Evidence** defines what to capture for proof and investigation  
  /governance/audit/

- **Security** is one domain within governance (threats, controls, resilience)  
  /governance/security/

- **Frameworks & Standards** provide reference mappings and terminology  
  /governance/frameworks/

---

## Start with these posts (foundation)

- {% post_url 2026-02-27-blast-radius-control-for-ai-agents %}
- {% post_url 2026-02-27-why-runtime-enforcement-beats-static-evaluation %}
- {% post_url 2026-02-27-cost-is-a-security-boundary %}
- {% post_url 2026-02-27-logging-is-not-governance %}
- {% post_url 2026-02-27-why-prompt-only-guardrails-fail %}

---

## Suggested future reading (when you add posts)

This hub becomes stronger as you add essays in these themes:

- **Risk taxonomy for agentic systems** (hazards, exposure, impact)
- **Residual risk and exception handling** (when and how to allow controlled violations)
- **Assurance artifacts** (what “proof” looks like for agent actions)
- **Kill-switch design patterns** (safe shutdown and containment)
- **Testing governance** (golden corpora, adversarial evaluation, replay)

_If you want, I can also draft the first post in this hub: “Residual Risk in Agentic AI: What to Accept, What to Block, and What to Escalate.”_
