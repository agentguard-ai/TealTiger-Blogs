---
title: "TealTiger v1.3.0: Autonomous Agent Governance — Cryptographic Proof for Every Decision"
description: "TealTiger v1.3 introduces Non-Human Identity management, cryptographic governance receipts, declarative workflows, behavioral drift detection, and OWASP Agentic Top 10 coverage. The most comprehensive open-source AI agent governance release yet."
tags: [release, governance, nhi, tealproof, tealflow, owasp, security, cost, drift, platform-adapters]
---

AI agents are no longer just answering questions. They're writing code, managing infrastructure, executing financial transactions, and making decisions that affect real people. The governance gap isn't theoretical anymore — it's operational.

TealTiger v1.3.0 closes that gap with **cryptographically verifiable governance** for autonomous AI agents. Every governance decision now produces a tamper-evident receipt. Every agent identity is managed as a first-class principal. Every policy is deterministic, traceable, and auditable.

This isn't incremental. v1.3 adds 7 new governance modules, 5 new providers, 3 platform adapters, and a complete evidence layer — while maintaining full backward compatibility with v1.2.

---

## The Core Problem v1.3 Solves

Most AI governance today falls into one of two traps:

1. **"Trust but verify" (too late)** — Log everything, review later. By the time you find the problem, the damage is done.
2. **"Block everything suspicious" (too noisy)** — Use an LLM to judge another LLM. Non-deterministic, slow, and expensive.

TealTiger takes a third path: **deterministic enforcement with cryptographic proof**. No LLM in the governance path. Same input + same policy = same decision, every time. And every decision produces a verifiable evidence trail.

---

## What's New in v1.3

### Non-Human Identity (NHI) Governance

Agents are principals, not ambient credentials. v1.3 introduces full identity lifecycle management:

- **Agent states**: active, suspended, revoked — with immediate enforcement on status change
- **Scope enforcement**: agents can only access resources within their declared scope
- **Zero Standing Privilege (ZSP)**: no permanent access. Agents get just-in-time grants that expire
- **Attestation**: verify agent integrity before granting access

```typescript
const engine = new TealEngineV13({
  nhi: {
    inventory: [
      {
        id: 'agent-billing-001',
        status: 'active',
        scopes: ['billing.read', 'billing.write'],
        environment: 'production',
        zsp: { enabled: true, max_grant_ttl: '5m' }
      }
    ]
  }
});
```

When an agent's status changes to `revoked`, all subsequent requests are immediately denied with `NHI_REVOKED` — no propagation delay, no cache invalidation needed.

### TealProof — Cryptographic Governance Receipts

Every governance decision now produces a cryptographic receipt:

- **Decision hash**: SHA-256(decision + context + timestamp + policy_version + prev_hash)
- **Merkle tree**: All decisions form a hash chain. Tampering with any decision invalidates the chain.
- **RFC 3161 timestamping**: Merkle roots are anchored to an external TSA on a configurable schedule
- **Governance Passport**: Per-agent summary of policy-compliant execution history

This means you can prove — cryptographically — that a specific governance decision was made, with a specific policy, at a specific time. Third-party auditors can verify this independently using the standalone Verification SDK (no TealTiger dependency required).

### TealFlow — Declarative Governance Workflows

Governance policies often need orchestration: "if this happens, then do that, but only if approved by someone with this role." TealFlow makes this declarative:

```yaml
name: high-risk-action-approval
on:
  agent_action:
    risk_score_above: 80

jobs:
  require-approval:
    steps:
      - name: Notify security team
        uses: tealtiger/notify-webhook
        with:
          url: ${{ secrets.SECURITY_WEBHOOK }}
      - name: Wait for approval
        uses: tealtiger/require-approval
        with:
          approvers: ['security-team']
          timeout: '30m'
```

Org-level workflows set a floor that team-level workflows cannot weaken. If a team tries to remove a control that the org requires, TealFlow rejects it with `WORKFLOW_FLOOR_VIOLATION`.

### TealClassifier — Local ML Without the Latency

v1.3 adds optional ML-based detection alongside the existing regex patterns:

- **ONNX Runtime**: Models run locally, no API calls, no data leaves the process
- **≤20ms inference**: CPU-only, ≤512 tokens
- **Ensemble modes**: `regex_only` (v1.2 behavior), `ml_only`, `ensemble_union` (block if either detects), `ensemble_intersection` (block only if both detect)
- **Graceful fallback**: If ONNX fails to load, reverts to `regex_only` with a `CLASSIFIER_FALLBACK` event

### Behavioral Drift Detection (TealDrift)

Agents change behavior over time — model updates, prompt drift, tool availability changes. TealDrift establishes statistical baselines and alerts when behavior diverges:

- Tracks: refusal rate, response length, topic distribution per agent/provider/model
- Configurable threshold (sigma-based)
- Requires minimum samples before alerting (prevents false alarms on cold start)

### Cost Governance That Code Can't Override (TealMonitor v2)

Application code can set budgets. But what if a developer raises the limit? TealMonitor v2 introduces **governance-owned ceilings**:

- Governance limits take precedence over application limits
- Application code cannot raise limits above the governance ceiling
- Anomaly detection: alerts when a request costs more than a configurable multiple of the rolling baseline
- Reasoning-token budgets: separate tracking for chain-of-thought tokens

### OWASP Agentic Top 10 — Zero Config

```typescript
const engine = new TealEngineV13({
  policy_packs: ['owasp-agentic-top10']
});
// That's it. All 10 ASI risks covered.
```

Each control maps to a specific OWASP category identifier, produces structured evidence, and can be extended without modifying shipped files.

### Platform Adapters

v1.3 ships native adapters for three enterprise agent platforms:

| Platform | Integration Style |
|----------|------------------|
| AWS Bedrock Agents | Lambda-backed guardrail adapter |
| AWS AgentCore | Pre/post action governance plugin |
| Azure AI Agent Service | Tool-call pipeline middleware |

All adapters use the same `TealEngineV13.evaluate()` internally — identical decisions regardless of platform.

### 12 LLM Providers

Five new providers join the existing seven: **DeepSeek**, **Groq**, **Together AI**, **HuggingFace TGI**, and **xAI (Grok)**. Total market coverage: 95%+.

---

## Architecture: How It Fits Together

```
Request → Pre-Evaluation Stage → Module Evaluation → Post-Evaluation Stage → Response
              │                        │                       │
              ├─ FREEZE check          ├─ TealGuard v2         ├─ TealProof (hash)
              ├─ PLAN_ONLY check       ├─ TealSecrets          ├─ TealAudit (TEEC)
              ├─ NHI validation        ├─ TealMonitor v2       ├─ Response hooks
              ├─ Attestation           ├─ TealDrift            ├─ OTel spans
              ├─ ZSP grant check       ├─ TealClassifier       ├─ TealFlow triggers
              └─ Scope/env check       └─ TealRegistry v2      └─ SIEM export
```

The pre-evaluation stage short-circuits on deny — if a FREEZE rule matches, nothing else runs. This guarantees that emergency controls always take precedence.

---

## Backward Compatibility

v1.3 is fully backward compatible with v1.2:

- All v1.2 policy configurations work without modification
- Identical Decision outputs for v1.2 configs
- No v1.3 features configured = v1.2 behavior
- All new config fields are optional with v1.2-equivalent defaults
- TealClassifier defaults to `regex_only`
- TealFlow is opt-in

---

## Install

```bash
npm install tealtiger@1.3.0
pip install tealtiger==1.3.0
docker pull tealtigeradmin/tealtiger-typescript:1.3.0
```

---

## What's Next: v1.4 — Zero-Config Adoption

v1.3 is the most comprehensive governance release we've built. v1.4 will focus on making it effortless to adopt:

- `observe()` mode — 1-line integration, instant visibility, no policies needed
- Progressive disclosure: observe → suggest → enforce
- Auto-baseline behavioral detection
- Framework adapters for LangChain, CrewAI, AutoGen, LlamaIndex

---

## Links

- [GitHub](https://github.com/agentguard-ai/tealtiger)
- [npm](https://www.npmjs.com/package/tealtiger)
- [PyPI](https://pypi.org/project/tealtiger/)
- [Discord](https://discord.gg/X2ePf8QAj)
- [LinkedIn](https://www.linkedin.com/company/tealtiger/)
- [X](https://x.com/TealtigerAI)
- [Bundle Manifest](https://github.com/agentguard-ai/tealtiger/blob/main/docs/bundle-manifest-v1.3.yaml)

---

*TealTiger is open source (Apache 2.0). Star us on [GitHub](https://github.com/agentguard-ai/tealtiger) if you believe AI agents need governance, not just guardrails.*
