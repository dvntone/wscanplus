# 🤖 AI Agent Collaboration Rules — wscan+

> **Universal guardrails for every AI agent contributing to this repository.**
> This applies to Claude, Gemini, Copilot/Codex, and any other LLM or automated agent.
> If any rule is unclear, **file a clarification issue — never guess or proceed uncertainly.**

---

## Context & Why These Rules Exist

This project is built entirely by AI agents under human oversight (review and direction only).
A previous project with unrestricted parallel agent work produced a build-success rate of ~30%
(1 of 3 apps built successfully). These rules exist to prevent that pattern from repeating.

**Priority order: Reliability → Traceability → Safety → Velocity.**
Velocity and parallelism are loosened only as empirical data proves sustained stability.

---

## 1. Single Work Unit at a Time

- Only **one open feature or bugfix PR** at any time across all agents.
- Every PR must reference exactly one GitHub Issue and clearly state its purpose in the PR description.
- **Never open a new PR while CI is failing.** Fix the broken build first, then continue.

## 1a. Merge Strategy — Squash Only

- **Only squash merge is enabled on this repository.** Merge commits and rebase are disabled.
- This is intentional and must not be re-enabled without explicit maintainer approval.
- **Why squash is required for AI-authored work:** Squash merge uses the GitHub web push API,
  which signs the resulting commit on behalf of the actor. Without squash, AI agent commits
  would be unsigned, breaking commit signing requirements and traceability.
- Agents must never instruct maintainers to use merge or rebase strategies.
- If you encounter a merge UI offering all three options, use **Squash and merge** only.

## 2. Keep Changes Small and Atomic

- One feature **or** one bugfix per PR — never combined.
- Prefer **< 200 lines changed** (tests excluded) per PR.
- No opportunistic refactors alongside feature/bug work.
  - Exception: a dedicated, test-only maintenance PR (see §7).

## 3. Dependency Management

- All dependency additions or version changes go in their **own dedicated PR** — never mixed with features or fixes.
- The PR description must include:
  - Why the dependency is needed.
  - License (must be OSS-compatible).
  - Any known security advisories.
- No dependency that introduces non-OSS or viral licenses without explicit maintainer approval.

## 4. Test Quality and Reporting

- Every code-changing PR must include or update **meaningful tests**.
  - No placeholder assertions such as `assert true`, `assertTrue(1 == 1)`, or empty test bodies.
- If a change causes test coverage to drop, flag it explicitly in the PR description.
- Agents should note when failure paths or edge cases are not yet covered.

## 5. Error Handling and Logs

- **Never catch and ignore exceptions silently.** Every caught error must be logged with enough
  context to reproduce and debug.
- If an unexpected error type appears during build, test, or runtime output, create a new GitHub
  Issue describing it before continuing work.

## 6. Design and Documentation Alignment

- After every **5 merged PRs**, one agent must open a "design checkpoint" issue that:
  - Summarizes what changed vs. original intent (using `docs/SESSION_STATE.md` as the reference).
  - Flags any architectural drift, feature creep, or policy violations.
  - Requests maintainer review if significant divergence is found.
- Any code change that affects public APIs, architecture, or user-facing behavior **must** include
  a corresponding documentation update in the same PR.
- If documentation update scope is unclear, file a doc-update issue rather than skipping it.

## 7. Scheduled Maintenance Sessions

- Every **5th PR** (counting from the last maintenance PR) must be a **maintenance-only session**:
  - Cleanup, deduplication, style harmonization, doc improvements.
  - No new features or behavior changes.
  - All existing tests must continue to pass.

## 8. Conflict and Escalation

- If two agents produce mutually blocking or repetitive PRs, the second agent must **close its own
  PR** and file an issue explaining the conflict before any further work.
- After **3 consecutive failed PRs** on the same issue or area, stop work and file an escalation
  issue tagged `agent-escalation` for maintainer review.
- Agents must always explain reasoning and cite which rule(s) guided decisions in PR descriptions.

## 9. Ethics and Security

- **Never commit secrets:** no API keys, tokens, passwords, or machine-specific config.
  - Android: use `local.properties` + secrets-gradle-plugin.
  - Desktop: use `.env` (git-ignored) + dotenv.
- **Never run the Electron app as root.**
- **Never use `exec()` with interpolated or template strings.** Use `spawn(cmd, [args])` only.
- **Validate every argument** passed to child processes: interfaces, file paths, ports, etc.
- No offensive capabilities (deauth injection, credential cracking, etc.) unless explicitly
  scoped and approved by the maintainer.

---

## Required Checks Before Marking a PR "Done"

| Platform | Commands |
|----------|----------|
| Android  | `./gradlew :core:test` and assemble debug/release as applicable |
| Node/Desktop | `npm test` and `npm run lint` |
| All | Confirm `.env` and `local.properties` are **not** tracked by git |

---

## Quick Reference Checklist (copy into every PR description)

```
- [ ] References exactly one GitHub Issue
- [ ] CI is green before this PR was opened
- [ ] One feature OR one bugfix (not both)
- [ ] < 200 LOC changed (tests excluded)
- [ ] No new dependencies mixed in (or this IS a dependency-only PR)
- [ ] Meaningful tests included/updated (no placeholder assertions)
- [ ] Errors are logged, not silently swallowed
- [ ] Documentation updated (or doc issue filed)
- [ ] .env / local.properties NOT committed
- [ ] No secrets of any kind committed
- [ ] Required platform checks passed (see table above)
- [ ] Merged via **Squash and merge** only (merge commits and rebase are disabled)
```

---

## Amending These Rules

These rules are strict and auditable. They may only be relaxed when:
1. Build/test success rate is consistently ≥ 80% over at least 10 PRs.
2. A maintainer explicitly approves the change in a dedicated issue.
3. The amended rule is committed in its own PR with clear rationale.

Do not unilaterally loosen or reinterpret any rule. File a clarification issue first.
