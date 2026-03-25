---
layout: page
title: "Evidence Record Schema (v1)"
---

This page defines the canonical **Evidence Record** structure used across TealTiger Governance.

Evidence Records capture **verifiable facts** about governance enforcement (policy evaluation, decisions, exceptions, and approvals).  
They are designed to support audit review, incident response, and control assurance.

> **Scope note:** This schema is compliance‑enabling. It provides evidence usable for audit and assurance, but does not by itself constitute a complete compliance program.

---

## Evidence Record (Canonical Fields)

An Evidence Record MUST be immutable once written. It MUST include the following canonical fields.

### Identity & Linkage

- `evidence_id` — unique identifier for this record
- `trace_id` — correlates all events within a single agent run / request
- `span_id` — optional sub-correlation identifier for a specific action
- `record_version` — schema version (e.g., `1.0`)

### Time

- `observed_at` — timestamp when the event occurred
- `recorded_at` — timestamp when the evidence was persisted

### Actor & Execution Context

- `actor` — identity claims for the initiating principal
  - `actor.subject` — stable principal identifier
  - `actor.issuer` — identity provider / trust root
  - `actor.auth_strength` — optional (MFA, workload identity, etc.)
- `execution` — context for where the action ran
  - `execution.environment` — prod/stage/dev
  - `execution.workload` — service/agent identifier
  - `execution.host` — optional runtime host/container/node

### Action & Resource

- `action` — what was attempted (e.g., `tool.invoke`, `data.read`, `model.generate`)
- `resource` — what it targeted (dataset/tool/endpoint/model)
  - `resource.type`
  - `resource.id`
  - `resource.attributes` — optional (classification, sensitivity, region)

### Governance Controls

- `control_id` — governance control identifier (stable)
- `policy_id` — evaluated policy identifier
- `policy_version` — evaluated policy version
- `decision` — `allow | deny | redact | warn | require_approval`
- `reason_code` — stable enum explaining decision
- `decision_details` — optional structured details (no free-form prose)

### Evidence Integrity

- `content_hash` — hash of the record body for tamper evidence
- `signature` — optional signature metadata
  - `signature.algorithm`
  - `signature.key_id`
  - `signature.value`

### Exceptions & Approvals (Optional)

- `exception_id` — if decision relied on an approved exception
- `approval_id` — if decision required approval/attestation
- `expiry_at` — for exceptions/approvals that expire

---

## Evidence Record (JSON Example)

```json
{
  "record_version": "1.0",
  "evidence_id": "evd_01J8Y8Q2M9G3F2K2XWZ7QZQ3Z9",
  "trace_id": "trc_01J8Y8Q1H2M0D4D8V7K2F8S9A1",
  "span_id": "spn_0007",
  "observed_at": "2026-03-25T09:12:40Z",
  "recorded_at": "2026-03-25T09:12:41Z",
  "actor": {
    "subject": "user:9c1b2d7a",
    "issuer": "entra-id",
    "auth_strength": "mfa"
  },
  "execution": {
    "environment": "prod",
    "workload": "agent:claims-triage",
    "host": "k8s/pod/claims-triage-7c4d"
  },
  "action": "tool.invoke",
  "resource": {
    "type": "tool",
    "id": "jira.create_ticket",
    "attributes": {
      "classification": "internal",
      "region": "in"
    }
  },
  "control_id": "SEC-TOOL-ALLOWLIST-001",
  "policy_id": "policy_tool_allowlist",
  "policy_version": "3.2.1",
  "decision": "deny",
  "reason_code": "TOOL_NOT_IN_ALLOWLIST",
  "decision_details": {
    "requested_tool": "jira.create_ticket",
    "allowed_tools": ["jira.search", "jira.comment"]
  },
  "content_hash": "sha256:9c8f4a2b...d17e",
  "signature": {
    "algorithm": "ed25519",
    "key_id": "kid_2026_01",
    "value": "MEUCIQD..."
  }
}
``
