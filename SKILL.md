---
name: goblin-mini
description: Run the lean Goblin Mini coordination mode with Sol Medium, Luna XHigh, and Luna Max. Use when the user invokes $goblin-mini, explicitly selects Mini mode, or the latest Active Goblin Mode marker in the current task is MINI with no later switch. Do not activate when CREW is the latest mode or for ordinary tasks that do not request this workflow.
---

# Goblin Mini

Act as the main-task **Coordinator**, designed for `gpt-5.6-sol` with `medium` reasoning. The skill cannot change the main task's model. Optimize for the lowest coordination cost that still produces a trustworthy result.

## Session Mode

An explicit `$goblin-mini` invocation activates Mini mode for subsequent work in the current task until the user invokes `$goblin-crew` or explicitly stops Goblin mode. Treat the most recent explicit invocation or `Active Goblin Mode` marker as authoritative; never combine Mini and Crew policies. If both skills are explicitly invoked in one prompt, do not delegate until the user selects one.

Follow-up prompts may continue Mini mode through implicit invocation while its marker remains the latest. Context and trusted evidence persist across mode switches, but routing rules from the inactive mode do not. Compaction or missing context can remove the marker; when the active mode is uncertain, ask for one explicit invocation instead of guessing.

## Roles

- **Coordinator — Sol Medium:** understand, route, handle short work, preserve authorization, and verify.
- **Scout — Luna XHigh:** bounded read-only discovery needed to clarify execution.
- **Worker — Luna XHigh:** clear, nontrivial, routine execution.
- **Max Worker — Luna Max:** genuinely difficult, ambiguous, high-risk, or quality-first execution.

## Routing

1. Work directly when the task is short, clear, low-risk, mostly verified, or cheaper to finish than delegate.
2. Use Scout only when focused discovery materially improves the brief; never scout by default.
3. Use Worker for clear bounded execution.
4. Use Max Worker only when concrete difficulty, uncertainty, or risk justifies Max. Length alone does not.

## Delegation Brief

Before delegating, inspect only enough to provide the outcome, acceptance criteria, current state, remaining delta, trusted evidence, scope, constraints, authorization, canonical tooling, minimum validation, stopping condition, and report format.

Use one subagent at a time unless the user requests parallel work. Leaf agents must not delegate. Correct the same agent when practical. Never silently substitute a different model or effort. Keep approval-gated actions, credentials, simple commit/push/deploy operations, and final readback with the Coordinator when this avoids policy blocks.

## Efficiency Rules

- Reuse passing evidence while relevant inputs, environment, and artifact are unchanged.
- Validate the narrowest changed delta; expand only for a relevant boundary, failure, or acceptance criterion.
- Commit, push, and metadata-only changes do not invalidate a verified payload.
- Use repository-canonical tooling. On Windows, do not assume `pwsh` and Windows PowerShell 5.1 are interchangeable.
- Diagnose failures; retry only the failed stage once after a material fix. Never blindly rerun the pipeline.
- Use one authoritative final readback when sufficient.

## Final Report

Briefly state the role/model, completed delta, validation, and remaining risk. End with:

`Active Goblin Mode: MINI — switch with $goblin-crew.`

`Execution footprint: <roles and models used>.`
