# wscan+ AI Agent Rules (must follow)

Full universal guardrails: [docs/AGENTS.md](docs/AGENTS.md)

## Approved Coding Agents

Only these agents may write or modify code, docs, and config in this repo:

- **Claude** (Anthropic) — primary coding agent
- **Copilot / Codex** (GitHub) — secondary coding agent
- **Gemini is NOT a coding agent for this repo.** See "AI Split" below.

## AI Split (Critical — do not change without maintainer approval)

| Role | AI | Where |
|------|----|--------|
| Coding agent | Claude / Copilot | PRs, commits, config |
| In-app threat analysis | Gemini / Vertex AI (Google AI Pro) | Android app only |
| Maps & location | Google Maps API | Android app only |

**Coding agents must never remove, replace, or modify the Gemini/Vertex integration in the Android app.** It is intentional and paid for. File an issue if you think something needs changing.

## PR / Issue Policy

- MAX 1 open PR at a time (total, across all agents)
- Every PR must reference exactly one GitHub Issue
- If CI is red: fix CI before any new PR or feature work

## Session Limits

- One feature OR one bugfix per session
- Keep PRs small: prefer < 200 LOC changed (tests excluded)
- No drive-by refactors

## Safety & Security

- Never commit secrets (API keys, tokens, credentials)
- Never run Electron app as root
- No `exec()` with interpolated strings — use `spawn(cmd, [args])` only
- Validate all args passed to child processes (interfaces, paths, ports)

## Required checks before "done"

| Platform | Commands |
|----------|----------|
| Android | `./gradlew :core:test` and assemble debug/release as applicable |
| Node/Desktop | `npm test` and `npm run lint` |
| All | Confirm `.env` and `local.properties` are not tracked by git |

## Traceability (required)

Every change must be traceable. This means:

- Branch names must include the agent prefix: `claude/`, `copilot/`, or `dvntone/`
- Commit messages must be descriptive (what changed and why — not just "update file")
- Every PR description must state: which agent made it, what was changed and why, and which rules guided the decisions
- No force-pushes. No amending commits after they are merged.
