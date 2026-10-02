---
name: workflow-hardening
description: Design observable, repeatable, and where useful blocking mechanisms for repeated, scalable, risky, failure-prone, provenance-sensitive, hard-to-reproduce, or expensive-to-verify execution. Use when manual verification or ad hoc prompting is no longer sufficient. Do not build a hardening mechanism for cheap one-off low-risk work.
---

# Workflow Hardening

Use when execution or evidence production is meaningfully:

- repeated;
- scalable;
- risky;
- failure-prone;
- provenance-sensitive;
- expensive to verify manually;
- difficult to reproduce;
- governed by important invariants;
- likely to recur.

Before building a new workflow-hardening capability, apply **capability-sourcing**.

This applies recursively to harness authoring itself. Inspect the live
repository's existing instructions, tests, gates, planning/evidence surfaces and
the target agent/runtime's mature customization mechanisms before inventing a
new harness structure.

Right-size the result. Prefer extending existing repository conventions over
creating a parallel operating system. Select the delivery surface by required
scope, loading behavior, authority, determinism, context cost, and runtime
support; keep procedures out of always-on context when conditional loading is
available, and move non-negotiable checks into deterministic enforcement when
the platform can support it.

## Objective

Convert important model claims, process states, and invariants into observable, repeatable, inspectable, and where useful blocking evidence.

Possible components:

- controlled inputs;
- fixtures / environment;
- source policy;
- runner / procedure;
- state tracking;
- guards;
- provenance;
- logging;
- sanity checks;
- validation rules;
- oracle / acceptance logic;
- retries;
- exception handling;
- human-review queues;
- evidence capture.

Prefer cheap rejection mechanisms before expensive verification.

For repeated defects or escaped regressions, ask:

> Why was this failure still representable, or why was this solved decision still open?

Escalate only as far as the evidence justifies:

1. fix the current instance;
2. detect recurrence;
3. encode the failure class as an invariant / contract;
4. remove unnecessary variation by reusing a proven baseline;
5. make the invalid state structurally impossible where practical.

When a useful closure is likely to recur, preserve its claim, scope, baseline, validity conditions, allowed variation, proof, and invalidation triggers in the lightest discoverable / enforceable mechanism that fits the risk.

## Automation amplification and containment

For repeated, scalable, scheduled, concurrent, retryable, fan-out, or externally
triggerable execution, inspect more than functional correctness.

Model the minimum useful exposure:

- **trigger surface** — who or what can enqueue the work;
- **unit side effect** — metered/scarce resource use, cash/quota use, external calls, writes, or privileged execution per run;
- **amplifiers** — frequency, concurrency, retries, fan-out, duplicates, stale work, and adversarial repetition;
- **persistence / privilege** — whether candidate or untrusted code can affect a reusable runner, credentialed environment, cache, or other durable state;
- **containment** — what bounds, cancels, isolates, rate-limits, budgets, or stops the exposure.

Stress the mechanism with the question:

> What happens if this valid-looking action executes 100x or 1000x more often than expected?

When the result can materially affect budget, runway, quota, availability,
security, privacy, or the parent Outcome, harden before scaling. Prefer the
cheapest reliable mechanism that closes the real path: trusted-trigger gates,
isolation / ephemeral execution, deduplication, cancel-superseded behavior,
concurrency / rate limits, timeouts, safe local reuse, quotas / budgets, stop
conditions, and resource telemetry.

Do not assume a cache or other conventional optimization is beneficial outside
the topology it was designed for. On persistent workers, verify whether local
state already provides the capability before adding repeated remote transfer.

For privileged self-hosted execution, treat untrusted candidate code as a trust
boundary, not merely another test input. A persistent runner that can execute
untrusted code can turn one accepted job into durable compromise.

Where mechanically testable, include a negative control for the amplified failure
class, not just a happy-path run.

## Enforcement graduation

For recurring or consequential closure, separate **Epistemic Closure** from
**Operational Closure**. Ask whether violation is mechanically decidable and,
if it is, whether a violating change/state can still silently pass. Prefer the
cheapest reliable guard: type/schema, canonical boundary, diff/path guard,
dependency rule, lint, contract/fixture/runtime test, or CI/policy gate.

A guard is not proven merely because it exists. Prove a known invalid case is
rejected, a representative allowed case is accepted, and—when blocking is the
purpose—that the guard is wired into the normal execution/merge path.

Prefer enforcing invariants over freezing implementation details. Protect exact
paths only when path ownership itself is the invariant.

If the outcome is not mechanically decidable, define the Human/Product gate and
observation surface. If hardening is deferred, record enforcement debt and do
not call a documentation-only rule Operationally Closed.

A sanity check can show that something is obviously wrong. It does not prove correctness.

## Restraint

Do not build infrastructure for a cheap one-off task.

If the hardening mechanism itself becomes sufficiently complex that its behavior is unclear, treat the hardening mechanism as the new system of interest and model it.

Do not recurse further unless a real problem requires it.
