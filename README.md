# Agent Cash Cow OS — Forecast Evidence public reference

[![Public Agent Runtime Reference](https://github.com/SamCT86/agent-forecast-foundry-case-study/actions/workflows/reference-tests.yml/badge.svg)](https://github.com/SamCT86/agent-forecast-foundry-case-study/actions/workflows/reference-tests.yml)

**Status:** runnable public engineering reference for Agent Cash Cow OS  
**Commercial product:** Agent Cash Cow OS  
**Production system:** private

Agent Cash Cow OS is one of the two commercial product tracks I present publicly.

The current product hypothesis centers on **forecast evidence and measurable decision advantage for autonomous agents**. This repository does not expose the private production OS. It publishes one bounded engineering pattern from that direction: an agent run should not be accepted merely because a model returned an answer.

A run can still be unsafe or unusable if it uses the wrong evidence, exceeds a cost or latency limit, returns incomplete provider state, or stores data that should not be persisted. This reference makes those checks reviewable in code.

## Start with the right proof surface

Agent Cash Cow OS now has three bounded public proof surfaces, and they answer different questions:

- **[Interactive Transaction Lab](https://www.sarmadtawfeek.com/agent-cash-cow)** — for a buyer or non-technical reviewer. It opens with a buyer-first 15-second timeout story and one-click guided failure run, then expands into synthetic scenarios for payment acknowledgement ambiguity, incomplete provider readback, replay risk and missing outcome evidence. No account or real money is required.
- **[Transaction reliability source proof](https://github.com/SamCT86/agent-cash-cow-os)** — for reviewers who want the synthetic transaction state machine, deterministic proof receipts, failure scenarios, tests and explicit public/private boundary in source form.
- **This GitHub repository** — for technical review of the separate forecast-evidence/runtime discipline: bound evidence, structured output, provider-state checks, cost/latency limits, replay/claim handling and deterministic acceptance.

None of these surfaces is the private production OS, and none proves paid adoption, forecast advantage, customer ROI or production-scale economics. Those remain separate evidence requirements.

If you are evaluating a concrete AI-agent workflow, use the Transaction Lab for the fast business-risk walkthrough, then inspect this repository for implementation-level proof.

## Commercial entry point

If you already run AI or integrations but do not fully trust their failure behavior, the closest current engagement is a **Reliability review**: failure, duplicate-action and handoff testing plus a prioritized action list.

If the business problem is still unclear, start with a **Workflow check**: establish a baseline, identify the biggest leak, estimate potential and recommend the next step.

Start with **2–3 sentences** about what is slow, expensive or unreliable. No technical brief or meeting is required to start, and no sensitive data should be sent yet. Scope and price are agreed before anything is ordered.

[Describe the workflow by email](mailto:sarmadtawfeek@gmail.com) · [See the current engagement options](https://www.sarmadtawfeek.com)

This entry path does not change the repository's evidence boundary: this reference still does **not** prove paid adoption, forecast advantage, customer ROI or production-scale economics.

## How a run moves through the reference

```text
request + allowed evidence
-> runId + request-fingerprint claim/replay guard
-> model/provider adapter
-> strict structured output
-> provider status + token/cost/latency data
-> deterministic verification
-> ACCEPTED / ABSTAINED / fail closed
-> sanitized journal
```

## Try it

Prerequisite: Node.js 22.

```bash
npm test
npm run eval
```

The default test and CI paths do **not** make a live model call or spend API budget. The provider boundary is injected so request shape, schema handling, telemetry and failure behavior remain deterministic in CI.

## What to inspect

- [`src/runtime-gate.mjs`](src/runtime-gate.mjs) — provider adapter, runtime checks, cost/latency accounting, sanitized journaling and eval logic.
- [`test/execution-path.test.mjs`](test/execution-path.test.mjs) — request boundary, persistence and transport-failure tests.
- [`test/runtime-gate.test.mjs`](test/runtime-gate.test.mjs) — post-model safety checks.
- [`eval/fixtures.mjs`](eval/fixtures.mjs) — synthetic eval cases.
- [`tools/run-eval.mjs`](tools/run-eval.mjs) — reviewer-facing eval command.
- [`PUBLIC_BOUNDARY.md`](PUBLIC_BOUNDARY.md) — what is public and what stays private.
- [`docs/VERIFICATION.md`](docs/VERIFICATION.md) — how stronger claims would need to be tested.
- [`docs/CONCURRENCY.md`](docs/CONCURRENCY.md) — the exact single-host claim/replay boundary and its non-guarantees.

## Failure cases this reference handles

The run fails closed when:

- output cites evidence that was not bound to the run;
- provider input tries to use an unbound reference;
- provider status is not complete;
- the provider request fails;
- latency or estimated cost exceeds the declared limit;
- a probability is invalid for the chosen decision state;
- hidden reasoning or secret-bearing fields would be persisted;
- structured output cannot be parsed under the expected contract;
- a persisted `runId` is reused with a different request fingerprint;
- an active or stale conflicting claim attempts to reuse the same `runId`.

An incomplete provider run is rejected before the journal is written. An exact sequential retry of an already-persisted `runId` returns the prior verified record without calling the provider again.

## What this proves — and what it does not

The fixtures test accepted, abstained and fail-closed behavior against synthetic records. They provide regression evidence for the published runtime pattern.

They do **not** prove forecast accuracy, production-scale reliability, commercial demand, paid adoption, or live-provider economics. Those claims require separate evidence.

## Public / private boundary

The private Agent Cash Cow OS repository contains the canonical product work and remains private.

Not published here:

- private prompts, orchestration and benchmark logic;
- live evidence or buyer data;
- production credentials;
- commercial controls;
- unreleased product and routing logic.

This public reference exists only to make a bounded part of the engineering approach inspectable.

## Related commercial work

- [Agent Cash Cow OS transaction reliability proof](https://github.com/SamCT86/agent-cash-cow-os) — synthetic payment-state reconciliation, replay containment and outcome-gated settlement proof.
- [MachineOutcome](https://github.com/SamCT86/machineoutcome-case-study) — verify observed outcome before trusting success or retry.
- [Portfolio](https://www.sarmadtawfeek.com) — live public overview of the current product and engineering proof surface.

MachineOutcome and Agent Cash Cow OS are the two commercial product tracks currently presented publicly.

## AI-native accountability

This reference is AI-assisted. My role is problem framing, system direction, acceptance criteria, testing, verification and final release judgment. It is not a claim that I manually wrote every line.
