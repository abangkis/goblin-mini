# Goblin Mini

Goblin Mini is a lean, cost-aware multi-agent coordination skill for Codex. It keeps simple work in the main task and uses only three optional delegated roles.

> Use the cheapest coordination path that still produces a trustworthy result.

## Roles

| Role | Model and effort | Purpose |
| --- | --- | --- |
| Coordinator | Sol Medium | Understand, route, handle short work, preserve authorization, and verify |
| Scout | Luna XHigh | Bounded read-only discovery needed to clarify execution |
| Worker | Luna XHigh | Clear, nontrivial, routine execution |
| Smart Worker | Luna Max | Genuinely difficult, ambiguous, high-risk, or quality-first execution |

Goblin Mini does not delegate merely because it was invoked. Direct execution remains the default for short, clear, low-risk, or already-verified work.

## Install

Ask Codex to install the skill from this repository:

```text
Use $skill-installer to install goblin-mini from
https://github.com/abangkis/goblin-mini
```

Alternatively, copy this repository into:

```text
~/.codex/skills/goblin-mini/
```

Start a new Codex task after installation so the skill catalog refreshes.

## Use

Start the main task with GPT-5.6 Sol at medium reasoning, then invoke:

```text
$goblin-mini

Complete the following task:
[describe the task, constraints, and desired outcome]
```

The task may be written in any language. The skill itself is written entirely in English and does not force the final response language.

## Efficiency Rules

- Do short work directly.
- Do not scout by default.
- Use one leaf subagent at a time unless parallel work is explicitly requested.
- Reuse trustworthy evidence while its relevant inputs remain unchanged.
- Validate the narrowest changed delta first.
- Use repository-canonical tooling.
- Retry only the failed stage once after a material fix.
- Use one authoritative final readback when sufficient.

## Usage and Cost Reporting

Goblin Mini always reports the roles and models actually used. It reports exact token usage only when authoritative usage metadata is available for every call, and currency cost only when authoritative current pricing is also available.

Local Codex skills may not receive per-call usage metadata. In that case Goblin Mini reports:

```text
Token cost: unavailable (usage metadata not exposed).
```

It never estimates or invents token usage or cost.

## Switching Coordination Skills

The recommended practice is to start a new Codex task when switching between [Goblin Mini](https://github.com/abangkis/goblin-mini) and [Goblin Crew](https://github.com/abangkis/goblin-crew). A fresh task gives the new skill a clean coordination context and makes its routing decisions easier to understand.

Switching skills inside an existing task is still possible: explicitly invoke the desired skill in a later prompt. Be aware that:

- The main task model does not change automatically.
- Existing context, evidence, and earlier routing decisions remain in the task and may influence the new route.
- Invoking both skills in the same prompt can create ambiguous routing because their policies overlap but select different Scout and Smart Worker configurations.

Use only one coordination skill per prompt. Prefer Goblin Mini for the leanest cost-aware route, and start a fresh task with Goblin Crew when stronger Sol-based exploration or strategic judgment is needed.

## Compatibility

Goblin Mini is designed around:

- `gpt-5.6-sol` with medium reasoning for the main Coordinator.
- `gpt-5.6-luna` with xhigh reasoning for Scout and Worker.
- `gpt-5.6-luna` with max reasoning for Smart Worker.

The main task model must be selected by the user or host. A skill cannot change its own main task model.

## License

MIT
