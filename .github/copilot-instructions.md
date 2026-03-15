# Copilot Instructions — wscan+

Read these files first:
- docs/SESSION_STATE.md
- AGENTS.md
- KNOWN_ISSUES.md (if present)
- docs/ROADMAP.md

Hard rules:
1. No secrets committed (ever).
2. One PR at a time; one Issue per PR.
3. Tests-first; CI must be green.
4. No shell injection risks: use spawn array form only; validate args.
5. Do not add new CI scanners (CodeQL/Sonar/Semgrep/etc.) unless explicitly requested.
6. Open all PRs as DRAFT (`gh pr create --draft`); mark 'Ready for Review' only when CI is green and all checklist items are complete.

If a request risks breaking builds or expands scope, STOP and file an Issue proposing a safe phased plan.
