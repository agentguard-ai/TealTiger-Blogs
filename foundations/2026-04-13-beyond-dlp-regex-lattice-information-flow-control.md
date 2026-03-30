---
title: "Beyond DLP Regex: Lattice-Based Information Flow Control for AI Agents"
description: Content scanning can miss multi-step leakage. Lattice-based information flow control enforces where data may flow—using labels, propagation, and provable constraints.
tags: [foundations, governance, runtime, security-theory, agents, data-security]

series: "Toward Provable AI Governance"
series_order: 6
status: design-notes
---

> **Status note:** This post explores scientific and architectural foundations for AI governance. It describes design direction and reasoning principles. **Not all mechanisms discussed are implemented in current TealTiger releases.**

---

## Why content scanning isn’t enough for agentic systems

Most “AI data protection” still looks like classic DLP:

- regex for identifiers
- keyword lists
- ML classifiers for sensitive topics
- redaction after the fact

These techniques help, but they focus on **surface content**.

Agentic systems introduce a harder problem: **information can be transformed across steps**.

A model can:

- read sensitive data
- summarize it
- translate it
- paraphrase it
- split it across tool calls
- output a “clean” version that contains no obvious signatures

This is not a rare corner case. It is the expected behavior of a capable agent.

So the governance question changes from:

> “Does the output contain a detectable pattern?”

to:

> “Is this output allowed to be derived from that input and released to this destination?”

That is an **information flow** question.

---

## The idea: control information flow, not just content

Information flow control (IFC) is a family of techniques used in high-assurance security systems.

Instead of scanning output strings, IFC tracks **where information came from** and enforces **where it may go**.

The classic foundation is a **security lattice**:

- every data source has a label (e.g., `public`, `internal`, `confidential`, `restricted`)
- every destination (channel) has a label
- the system enforces permitted flows between labels

This is why IFC can stop “laundering”:

- if an agent reads `restricted` data,
- then any derived artifact remains `restricted` unless explicitly downgraded via a controlled process.

---

## Lattices in one sentence

A lattice is a partially ordered set where you can compute:

- whether a flow is allowed (`canFlow(source, destination)`)
- the combined sensitivity of two inputs (`join` / least upper bound)

In practice:

- **Labels** define sensitivity
- **Order** defines allowed flow
- **Join** defines how labels propagate when data is combined

---

## A concrete, practical lattice for AI governance

A minimal lattice many teams can adopt:

- `public` < `internal` < `confidential` < `restricted`

And then define channels:

- Slack public channel → `public`
- internal ticket system → `internal`
- customer support console → `confidential`
- finance or HR systems → `restricted`

Now an agent that reads `restricted` data should not be able to output to a `public` channel.

This is deterministic, explainable, and auditable.

---

## Why label propagation matters in agent workflows

Agentic systems create derived artifacts constantly:

- retrieval results
- tool outputs
- intermediate summaries
- final responses

If you don’t propagate labels, you’re stuck with “scan the output” again.

With propagation:

- if any input is `confidential`, the combined working set becomes `confidential`
- summaries inherit the label
- outputs inherit the label

This makes policy enforcement simple:

> Don’t allow outputs to flow to a destination with a lower label.

---

## Preventing the three big leakage modes

### 1) Direct leakage
Agent reads sensitive content and outputs it.

- DLP may catch it.
- IFC always catches it because the label forbids the flow.

### 2) Transformed leakage (laundering)
Agent reads sensitive content and outputs a paraphrase.

- DLP often misses.
- IFC blocks because the derived output retains the high label.

### 3) Multi-step leakage across tools
Agent reads sensitive content, sends it to a tool, receives a reformatted version, then outputs it.

- DLP is brittle.
- IFC follows the flow through tool calls and preserves classification.

---

## How this fits TealTiger’s deterministic-first posture

Lattice-based flow control is a natural match for governance that aims to be:

- deterministic
- fail-closed
- evidence-driven

It provides a crisp enforcement rule:

- **allow** if `canFlow(label_of_data, label_of_destination)` is true
- **deny** otherwise

No probabilities. No guessing.

---

## What about exceptions (downgrades)?

Enterprises do sometimes need controlled downgrades:

- publishing a sanitized report
- sharing aggregated metrics
- responding to a customer with specific data

IFC handles this by treating downgrades as **explicit, governed actions**:

- require approval
- record justification
- attach evidence of sanitization
- produce an audit trail

In other words:

> If data is moving “down” the lattice, it must be deliberate and reviewable.

---

## Evidence implications (what to record)

A lattice-based governance engine can produce extremely strong evidence.

A defensible evidence record should include:

- the lattice definition version (labels and order)
- labels assigned to inputs and tool outputs
- label propagation steps (`join` events)
- attempted destination and its label
- the `canFlow` decision and the rule that produced it
- any downgrade approvals (if applicable)

This gives auditors what they want:

- what data was involved
- how it was classified
- where it attempted to go
- why it was allowed or denied

---

## What this does *not* claim

To be explicit:

- Lattice labels do not magically appear—you need classification.
- Some classification requires organizational policy.
- IFC does not replace content inspection; it complements it.

The point is that IFC gives you a deterministic backbone.

Content scanning becomes a secondary layer, not the only line of defense.

---

## What’s next in the series

Information flow control addresses *where data may go*.

The next question is *how posture adapts* as evidence accumulates:

- unknown agents vs trusted agents
- suspicious runs tightening enforcement
- explainable gradual changes

That leads to **Bayesian trust scoring**—updating trust based on observations, without turning governance into a black-box risk score.

---

*This post is part of the* **Toward Provable AI Governance** *series.*
