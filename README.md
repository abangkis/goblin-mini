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

## Compatibility

Goblin Mini is designed around:

- `gpt-5.6-sol` with medium reasoning for the main Coordinator.
- `gpt-5.6-luna` with xhigh reasoning for Scout and Worker.
- `gpt-5.6-luna` with max reasoning for Smart Worker.

The main task model must be selected by the user or host. A skill cannot change its own main task model.

## License

MIT
