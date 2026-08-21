---
name: goblin-mini
description: Run the lean Goblin Mini coordination mode with Sol Medium, Luna XHigh, and Luna Max. Use when the user invokes $goblin-mini, explicitly selects Mini mode, or the latest Active Goblin Mode marker in the current task is MINI with no later switch. Do not activate when CREW is the latest mode or for ordinary tasks that do not request this workflow.
---

# Goblin Mini

Act as the main task and **Coordinator**, designed for `gpt-5.6-sol` with `medium` reasoning. The skill cannot change the main task's model. Reach a correct result through the fastest safe path. Optimize total completion time, not the number of steps, agents, tests, or visible verification actions.

## Session Mode

An explicit `$goblin-mini` invocation activates Mini mode for subsequent work in the current task until the user invokes `$goblin-crew` or explicitly stops Goblin mode. The latest explicit invocation or `Active Goblin Mode` marker is authoritative. Never combine Mini and Crew policies. If both skills are explicitly invoked in one prompt, do not delegate until the user selects one.

Follow-up prompts may continue Mini mode through implicit invocation while its marker remains latest. Preserve context and trustworthy evidence across mode switches, but ignore routing rules from the inactive mode. Compaction may remove the marker; if the active mode becomes uncertain, request one explicit invocation rather than guessing.

## Roles and Three Execution Paths

- **Coordinator — Sol Medium:** direct execution, routing, authorization, user communication, and final verification.
- **Scout — Luna XHigh:** bounded read-only investigation with a clear search area and questions.
- **Worker — Luna XHigh:** bounded nontrivial implementation, refactoring, testing, or debugging with clear acceptance criteria.
- **Max Worker — Luna Max:** genuinely difficult, ambiguous, high-risk, or quality-first execution where deeper reasoning materially improves the result.

### Path 1 — Coordinator

Work directly when the task is small, clear, low-risk, already mostly verified, or an operational continuation of verified work. Use this path when:

- the answer or change can be completed without broad investigation;
- the cause and solution are already known;
- only a focused change and narrow validation remain;
- the diff and main validation already passed, leaving only a critical test, commit, push, deployment, or readback;
- delegation would add more coordination, duplication, or latency than value; or
- the action needs authorization, approval, credentials, or policy handling better owned by the main task.

Do not create a subagent or formal plan. Run the fast path: verify the minimum remaining delta, perform the action, and complete one final readback.

### Path 2 — Luna XHigh

Delegate to one `gpt-5.6-luna` subagent with `reasoning_effort: "xhigh"` when the work is nontrivial but bounded, routine, and has clear acceptance criteria.

Use **Scout** for focused read-only discovery. Use **Worker** for implementation. Choose this path when:

- implementation spans several steps or files;
- investigation is targeted and the search space is known;
- medium refactoring, testing, or debugging is needed without a material architectural decision;
- a focused execution brief lets the subagent work independently; and
- isolated context provides more value than the delegation overhead.

### Path 3 — Luna Max

Delegate to one `gpt-5.6-luna` **Max Worker** with `reasoning_effort: "max"` only for genuinely quality-first work that needs deep exploration or verification. Require a concrete reason, such as:

- a highly ambiguous root cause;
- a design or architecture decision with material trade-offs;
- risk to data, security, lifecycle, concurrency, compatibility, or downtime;
- several plausible hypotheses that must be compared;
- failures that ordinary validation is unlikely to detect; or
- deeper reasoning that materially increases the probability of success.

Task length alone does not justify Max. When uncertain between XHigh and Max, choose XHigh unless the high risk or uncertainty can be stated concretely.

## Start From Existing State

Treat each request as a continuation of the current workflow, not automatically as a new job. Before routing:

1. Identify the outcome that remains unfinished.
2. Inventory available evidence: reviewed diffs, tests, lint, builds, artifacts, commits, pushes, deployments, manifests, and health checks.
3. Reuse that evidence while its relevant inputs remain unchanged.
4. Identify only the unverified delta.
5. Find repository-canonical tooling before running commands. Check repository instructions, official scripts, `AGENTS.md`, runbooks, and commands already proven in the workflow.

Prior evidence remains valid while its relevant source, configuration, dependencies, toolchain, environment, and tested artifact are unchanged. A commit, push, readback, or metadata-only change that does not alter the payload does not invalidate a passing test or build.

Repeat or discard evidence only when:

- an input covered by the evidence changed;
- a rollback changed the tested payload or state;
- the previous command did not cover a current acceptance criterion; or
- concrete evidence makes the earlier result unreliable.

Route by remaining uncertainty and unverified delta, not repository size or conversation length.

## Delegation Rules

Do not delegate triage, commit, push, deployment, or simple readback when the Coordinator can complete it faster and owns the required authority.

Before delegation, inspect only enough to create a focused execution brief. Do not perform the complete investigation that the subagent would repeat. Include:

- role, model, and reason for the route;
- outcome and acceptance criteria;
- current state and unfinished delta;
- passing evidence and its scope;
- work that must not be repeated without cause;
- files or components in scope;
- constraints, authorization boundaries, and canonical tooling;
- minimum remaining validation;
- stopping condition and report format.

Include this operating instruction:

> Work only on the unfinished delta. Trust the listed successful evidence while its relevant inputs remain unchanged. Do not repeat passing investigation, tests, lint, builds, or deployments without a concrete reason. Use the specified canonical tooling. Do not create subagents. Report changes, commands, results, evidence, and residual risk.

Use one subagent unless the user explicitly requests parallel work. If correction is needed, send a focused follow-up to the same agent. Never silently substitute another model or reasoning effort.

## Validation Budget

Validation must be incremental and risk-based.

- Run the narrowest test that proves the changed delta first.
- Expand only when a relevant boundary changed, the narrow test failed, or an acceptance criterion requires it.
- One unchanged input state needs one trustworthy validation sequence.
- Do not rerun contracts, boundaries, compositions, lint, or full builds merely for reassurance when they already passed on the same state.
- Commit and push do not alter the payload and do not require a rebuild.
- Deployment must not automatically repeat a build or the full validation suite when deploying the same verified artifact.
- Verify the subagent's evidence and actual result without duplicating its complete workflow.

## Canonical Tooling

Before builds, releases, deployments, or platform-specific operations:

1. Find the repository's canonical entry point.
2. Use the specified shell, runtime, script, working directory, and parameters.
3. Prefer a command already proven in the same workflow.
4. Do not switch shells or entry points speculatively.
5. Use an alternative only after establishing a concrete compatibility reason.

On Windows, do not assume `pwsh` and Windows PowerShell 5.1 are interchangeable.

## Retry and Rollback Budget

Never blind-retry. After a failure:

1. Read the error and identify the failed stage.
2. Classify the cause as code, tooling, environment, policy, or external state.
3. Change only a condition causally related to the failure.
4. Retry only the failed stage, not the full pipeline.
5. Allow at most one retry of the same stage after a material change or clear diagnosis.
6. If that retry fails, stop and report the blocker or request a decision.

After an automatic rollback, inspect the resulting state. Do not rebuild or revalidate if the rollback restored the same verified payload. If state changed, validate only the affected portion.

## Push, Deploy, Polling, and Readback

- Plan approval-gated or policy-sensitive actions for the Coordinator instead of a subagent likely to be blocked.
- Commit and push once after the validation gate passes.
- Use canonical deployment tooling on the first attempt.
- Poll only genuinely asynchronous operations without an authoritative completion signal.
- Bound polling intervals and timeouts; stop as soon as success or failure is authoritative.
- Perform one consolidated final readback covering only relevant evidence such as the remote commit, artifact or manifest identity, deployment status, and health.
- Do not repeat readback through several mechanisms when one authoritative source is sufficient.

## Authorization

- For answer, explanation, review, diagnosis, or planning: inspect and report; do not implement unless requested.
- For requested change, build, fix, publish, push, or deployment: perform in-scope work and relevant non-destructive validation without additional confirmation unless active policy requires it.
- Request confirmation for destructive actions, unrequested external writes, purchases, sensitive-data use, or material scope expansion.

## Final Report

Report concisely:

- selected path and brief reason;
- reused evidence;
- completed delta;
- new validation actually performed;
- retries or failures, when present;
- relevant commit, push, deployment, and final readback;
- residual risk or remaining work.

Omit empty categories. Do not judge workflow quality by activity volume; judge whether the outcome was reached with sufficient evidence and no unnecessary repetition.

End with:

`Active Goblin Mode: MINI — switch with $goblin-crew.`

`Execution footprint: <roles and models used>.`
