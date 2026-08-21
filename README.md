# Goblin Mini

Goblin Mini is a lean, cost-aware multi-agent coordination skill for Codex. It keeps simple work in the main task and uses only three optional delegated roles.

> Use the cheapest coordination path that still produces a trustworthy result.

## Roles

| Role | Model and effort | Purpose |
| --- | --- | --- |
| Mini Coordinator | Sol Medium | Understand, route, handle short work, preserve authorization, and verify |
| Pathfinder | Luna XHigh | Bounded read-only discovery needed to clarify execution |
| Runner | Luna XHigh | Clear, nontrivial, routine execution |
| Bruiser | Luna Max | Genuinely difficult, ambiguous, high-risk, or quality-first execution |

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

## Session Mode and Switching

Invoke Goblin Mini once to activate Mini mode for subsequent work in the same task:

```text
$goblin-mini
Complete the following work: ...
```

Later prompts can focus on the work without repeating the invocation. Goblin Mini marks each final report with:

```text
Active Goblin Mode: MINI — switch with $goblin-crew.
```

Invoke `$goblin-crew` once to switch modes. The latest explicit invocation or mode marker wins; the policies are never combined. Existing context and trustworthy evidence remain available across switches, but the inactive routing policy is ignored. If compaction removes the marker, invoke the desired skill once again.

Do not invoke both skills in one prompt. A fresh task remains useful when you want completely clean context, but it is not required for ordinary mode switching.

## Compatibility

Goblin Mini is designed around:

- `gpt-5.6-sol` with medium reasoning for the Mini Coordinator.
- `gpt-5.6-luna` with xhigh reasoning for Pathfinder and Runner.
- `gpt-5.6-luna` with max reasoning for Bruiser.

The main task model must be selected by the user or host. A skill cannot change its own main task model.

## License

MIT
