🤖 AI Agent Collaboration Rules — wscan+

Universal guardrails for every AI agent contributing to this repository.

## Approved Coding Agents

Only these agents may write or modify code, docs, and config:
- **Claude** (Anthropic) — primary coding agent
- - **Copilot / Codex** (GitHub) — secondary coding agent
 
  - **Gemini is NOT a coding agent for this repo.** See AI Split section.
 
  - ## AI Split (Critical — do not change without maintainer approval)
 
  - | Role | AI | Where |
  - |------|----|--------|
  - | Coding agent | Claude / Copilot | PRs, commits, config |
  - | In-app threat analysis | Gemini / Vertex AI (Google AI Pro) | Android app only |
  - | Maps & location | Google Maps API | Android app only |
 
  - Coding agents must never remove, replace, or modify the Gemini/Vertex integration in the Android app. It is intentional, already paid for via Google AI Pro, and required for Play Store distribution. File an issue if you believe something needs changing — do not act unilaterally.
 
  - ## Context & Why These Rules Exist
 
  - This project is built by AI agents under human oversight (review and direction only). A previous project with unrestricted parallel agent work and no clear AI boundaries produced a build-success rate of ~30%. These rules exist to prevent that pattern from repeating.
 
  - **Priority order: Reliability → Traceability → Safety → Velocity.**
 
  - ## 0. Traceability (new — required for every change)
 
  - Every change must be fully traceable back to the agent that made it:
 
  - - Branch names must start with the agent prefix: `claude/`, `copilot/`, or `dvntone/`
    - - Commit messages must describe what changed and why — not just "update file" or "fix"
      - - Every PR description must include:
        -   - Which agent made it
            -   - What was changed and why
                -   - Which rule(s) guided the decisions
                    - - No force-pushes to any branch
                      - - No amending commits after they are merged
                       
                        - ## 1. Single Work Unit at a Time
                       
                        - - Only one open feature or bugfix PR at any time across all agents
                          - - Every PR must reference exactly one GitHub Issue
                            - - Never open a new PR while CI is failing — fix the broken build first
                             
                              - ## 2. Keep Changes Small and Atomic
                             
                              - - One feature or one bugfix per PR — never combined
                                - - Prefer < 200 lines changed (tests excluded) per PR
                                  - - No opportunistic refactors alongside feature/bug work
                                    - - Exception: a dedicated, test-only maintenance PR (see §7)
                                     
                                      - ## 3. Dependency Management
                                     
                                      - - All dependency additions or version changes go in their own dedicated PR
                                        - - The PR description must include: why it's needed, its license, any known security advisories
                                        - No dependency that introduces non-OSS or viral licenses without explicit maintainer approval

                                        ## 4. Test Quality and Reporting

                                        - Every code-changing PR must include or update meaningful tests
                                        - - No placeholder assertions (assert true, assertTrue(1 == 1), empty test bodies)
                                          - - If a change causes test coverage to drop, flag it explicitly in the PR description
                                           
                                            - ## 5. Error Handling and Logs
                                           
                                            - - Never catch and ignore exceptions silently
                                              - - Every caught error must be logged with enough context to reproduce and debug
                                                - - If an unexpected error appears during build, test, or runtime, create a GitHub Issue before continuing

                                                ## 6. Design and Documentation Alignment

                                                - After every 5 merged PRs, one agent must open a "design checkpoint" issue comparing current state to docs/SESSION_STATE.md
                                                - - Any change that affects public APIs, architecture, or user-facing behavior must include a documentation update in the same PR
                                                 
                                                  - ## 7. Scheduled Maintenance Sessions
                                                  - 
                                                  - Every 5th PR must be maintenance-only: cleanup, deduplication, style, doc improvements
                                                  - - No new features or behavior changes in maintenance PRs
                                                    - - All existing tests must continue to pass

                                                    ## 8. Conflict and Escalation

                                                    - If two agents produce conflicting PRs, the second must close its own and file an issue
                                                    - - After 3 consecutive failed PRs on the same area, stop and file an escalation issue tagged `agent-escalation`
                                                     
                                                      - ## 9. Ethics and Security
                                                     
                                                      - - Never commit secrets: no API keys, tokens, passwords, or machine-specific config
                                                        - Android: use `local.properties` + secrets-gradle-plugin
                                                        -   - Desktop: use `.env` (git-ignored) + dotenv
                                                            - - Never run the Electron app as root
                                                              - - Never use exec() with interpolated strings — use spawn(cmd, [args]) only
                                                                - - Validate every argument passed to child processes
                                                                  - - No offensive capabilities (deauth injection, credential cracking, etc.) without explicit maintainer approval
                                                                   
                                                                    - ## Required Checks Before Marking a PR "Done"
                                                                   
                                                                    - | Platform | Commands |
                                                                    - |----------|----------|
                                                                    - | Android | `./gradlew :core:test` and assemble debug/release as applicable |
                                                                    - | Node/Desktop | `npm test` and `npm run lint` |
                                                                    - | All | Confirm `.env` and `local.properties` are not tracked by git |
                                                                    - 
                                                                    ## Quick Reference Checklist (copy into every PR description)

                                                                    - [ ] Made by: [Claude / Copilot / dvntone]
                                                                    - [ ] - [ ] References exactly one GitHub Issue
                                                                    - [ ] CI is green before this PR was opened
                                                                    - [ ] - [ ] One feature OR one bugfix (not both)
                                                                    - [ ] < 200 LOC changed (tests excluded)
                                                                    - [ ] - [ ] No new dependencies mixed in (or this IS a dependency-only PR)
                                                                    - [ ] - [ ] Meaningful tests included/updated
                                                                    - [ ] - [ ] Errors are logged, not silently swallowed
                                                                    - [ ] - [ ] Documentation updated (or doc issue filed)
                                                                    - [ ] - [ ] `.env` / `local.properties` NOT committed
                                                                    - [ ] - [ ] No secrets of any kind committed
                                                                    - [ ] - [ ] Required platform checks passed
                                                                    - [ ] - [ ] Gemini/Vertex integration untouched (or issue filed if change needed)
                                                                   
                                                                    - [ ] ## Amending These Rules
                                                                   
                                                                    - [ ] Rules may only be relaxed when:
                                                                    - Build/test success rate is consistently ≥ 80% over at least 10 PRs
                                                                    - A maintainer explicitly approves the change in a dedicated issue
                                                                    - - The amended rule is committed in its own PR with clear rationale
