# claude-bugbash

Claude Code plugin for **bug bashing**: exercise the **running** product first, use code **second** to triage. Charter: **find and reproduce** with evidence, not to own fixes unless you ask.

## Install

```text
/plugin marketplace add riwaht/claude-bugbash
/plugin install bugbash@claude-bugbash
```

## Commands

| Command | Role |
|---------|------|
| `/bug-bash` | Main entry; optional `[quick\|standard\|deep]` then **scope**. |
| `/bug-bash-quick` | **`quick`** intensity shortcut. |
| `/bug-bash-deep` | **`deep`** intensity shortcut. |

## Skills

| Skill | Role |
|-------|------|
| `using-bugbash` | **When** Bugbash applies; philosophy; how to split subagents. |
| `bugbash-core` | Mission brief, intensity, **risk triage**, phases, evidence bar, flaky table, coverage summary, JSON export. |

## Subagents

| Agent | Role |
|-------|------|
| `bugbash-hunter` | Generalist for **mixed** API + UI in a **small** scope. |
| `bugbash-api` | Contracts, auth, idempotency, concurrency on **machine-facing** surfaces. |
| `bugbash-ui` | Flows, forms, navigation, client-visible state; light a11y smoke only. |

## Optional: nudge Claude for QA wording only

Add to `CLAUDE.md` if you want Bugbash considered whenever you **talk like QA**—not for every coding message:

*When I ask for QA, stress testing, or a bug bash, use the Bugbash plugin skills; stay in find-and-reproduce mode until I ask for fixes.*

## License

MIT — see [`license.txt`](license.txt).

## Privacy

See [`PRIVACY.md`](PRIVACY.md) for how this project handles data (high level: no maintainer-operated analytics; third parties like your Claude provider and git host have their own policies).
