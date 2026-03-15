# wscan+ AI Agent Rules (must follow)

> Full universal guardrails: **[docs/AGENTS.md](docs/AGENTS.md)**
> All agents (Claude, Gemini, Copilot/Codex, etc.) must read and follow that document.

## PR / Issue Policy
- MAX 1 open PR at a time (total).
- Every PR must reference exactly one GitHub Issue.
- If CI is red: fix CI before any new PR or feature work.
- **Merge strategy: Squash and merge only.** Merge commits and rebase are disabled.
  Squash uses the GitHub web push API, which signs commits — required for all AI-authored work.

## Session Limits
- One feature OR one bugfix per session.
- Keep PRs small: prefer <200 LOC changed (tests excluded).
- No drive-by refactors.

## Safety & Security
- Never commit secrets (API keys, tokens, credentials).
- Never run Electron app as root.
- No `exec()` with interpolated strings. Use `spawn(cmd, [args])` only.
- Validate all args passed to child processes (interfaces, paths, ports).

## Required checks before "done"
- Android changes: `./gradlew :core:test` and assemble debug/release as applicable
- Node/Desktop changes: `npm test` and `npm run lint`
- Confirm `.env` and `local.properties` are not tracked by git
