Absolutely! Here’s how to best archive and preserve this historic Copilot Chat session for deep research, agent onboarding, and phase planning. This version is optimized for cross-tool analysis and will be easy to grep, index, and consume by humans or AI (Codex, Copilot, etc.).  
**Instructions:**  
- Save this as `docs/PLANNING_SESSION_2026-03-10.md` in your new repo (`wscanplus`)  
- If you want plain text or another format, just ask!

---

# 💬 PLANNING SESSION 2026-03-10

**Source Repos:**  
- Old: [dvntone/wifisentry](https://github.com/dvntone/wifisentry)  
- New: [dvntone/wscanplus](https://github.com/dvntone/wscanplus)  

---

## 1. SESSION CONTEXT

- **Goal:** 
  - Professional-grade wireless threat detection via Android + Linux desktop hub ("wscan+")
- **Constraints:** 
  - Security/audit focus.  
  - No hardcoded secrets.  
  - Linux-first (Windows/Flatpak/Snap out of scope).  
  - Must enforce robust AI guardrails and one-PR safety.
- **Session method:** 
  - Multi-part, deep research and strategic planning, covering architecture, best practices, and stepwise roadmapping for AI/agent infrastructure.

---

## 2. RAW DECISION LOG & STRATEGIC FINDINGS

---

### Repo, Platform, and Roadmap Decisions

- **Repo:** Move from `wifisentry` to new `wscanplus`, public, GPL-3.0, clean slate.
- **Audience:** Power users, IT, security professionals; no “lowest common denominator.”
- **Android:** Standalone WiFi scanner, supports indirect (VPN-based) deauth detection; uses biometric lock and audit logs.
- **Desktop:** Linux Electron hub, supports both passive (node-wifi) and monitor mode (aircrack-ng/iw).  
- **Mapping:** Use Google Maps SDK (Android) for visualizing scan history and threat heatmaps. Drop WiGLE (privacy and scope).
- **Export:** Always include PCAP, JSON. No obscure formats unless directly needed.
- **Kismet:** Useful as optional backend and especially for GPS "phone as sensor" mode (HTTP bridge from Android’s GPS).
- **Secrets:** 
    - Android: `secrets-gradle-plugin` with secrets in `local.properties` (excluded by `.gitignore`).
    - Desktop: `.env` file (excluded), example provided.
    - Never hardcode, and always validate with TruffleHog or similar CI check.
- **PR/AI guardrails:**
    - Only one PR at a time, one issue per PR.
    - Limit session size, require test-first, and enforce “fix CI first” before features.
    - If a change will break build, AI must stop and propose via Issue.
    - All critical context is persisted in docs (`SESSION_STATE.md`, `AGENTS.md`).

### CI/CD, Coding, and Toolchain

- **CI Tests:**  
    - Keep only essential: Android (build + unit tests + lint), Node (test + lint), secret scan.  
    - Drop CodeQL, SonarCloud, Semgrep, Detekt unless justified by Issue.
    - Use timeouts in CI to prevent agent hang.
- **Toolchain:**  
    - Desktop: Node.js 20.x, npm 10.x, Electron, JDK 17, Git 2.40+, aircrack-ng 1.7+, iw, adb, tshark 4.0+.
    - Android: JDK 17, Android Studio (or command-line tools).
- **.gitignore:**  
    - Must exclude `.env`, `local.properties`, `node_modules`, build directories.
- **Recovery:**  
    - Session error handling: auto-log last step, close or finish incomplete PRs, never open two PRs for one Issue.
- **Session leads with test manifest**

---

## 3. FULL ROADMAP PHASES (As captured in this session)

**Phase 0:** Foundation, hygiene, guardrails, minimal stable CI, branch protection

**Phase 1:** Android: core scanner, Google Maps threat map, VPN anomaly detect, biometrics, PCAP/JSON export.

**Phase 2:** Linux desktop core: passive/active/monitor scanning, dual adapter/tether support, PCAP analysis.

**Phase 3:** Android ↔ Desktop: ADB/WS sync, remote trigger/scan, timeline/fused data.

**Phase 4:** AI: Gemini analysis, prompt templates, Gemini key proxy, AI-driven threat scoring.

**Phase 5:** Polish, distribution, docs, signed APK, AppImage, dependency setup wizard.

---

## 4. SESSION EXCERPTS & CRITICAL RECOMMENDATIONS

**On Token/Security Handling:**  
- “ProGuard/R8 does NOT hide secrets. Use secrets-gradle-plugin/buildConfig with .gitignore, never log or expose; for production, proxy everything requiring secrets away from device if possible.”

**On CI Setup:**  
- “Minimal checks: `/gradlew :core:test` and `npm test`. Lint on both. Add TruffleHog or similar for secret scanning. Drop anything with high config/false-positive burden unless proven valuable.”

**On Mapping:**  
- “Google Maps SDK on Android is ideal: pair WifiManager scan with FusedLocationProvider, plot on map as markers/heatmap by signal/threat. Color code pins. Keep export to PCAP for raw data.”

**On Kismet:**  
- “Kismet’s best value: Android acts as GPS sensor—run a lightweight HTTP server app on phone, desktop Kismet pulls GPS over LAN, perfect for wardrive/survey use.”

**On AI/PR Management:**  
- “One PR at a time, tests first, CI green before feature, never more than 200 LOC/PR unless tests, and all secrets policies and shell safety rules (spawn, never exec) must be auto-enforced via Copilot/copilot-instructions.”

---

## 5. FILES AND BOILERPLATE CREATED FOR RESUMPTION

- `docs/SESSION_STATE.md`: Full session state, architectural decisions, next steps.
- `AGENTS.md`: AI contributor and workflow guardrails.
- `.github/copilot-instructions.md`: Copilot/Codex/Auto prompt reference for project onboarding.
- `docs/ROADMAP.md`: Full phased plan.
- `docs/DEPENDENCIES.md`: Tool and SDK versioning.
- `.env.example`, `secrets.defaults.properties`: Secrets scaffolding.
- Updated `.gitignore`.

---

## 6. OUTSTANDING ACTIONS / FLAGS

- [ ] Archive or freeze `wifisentry` repo; move all future commits to `wscanplus`.
- [ ] Add all above docs, primes Issues board via “Phase 0: add guardrails/docs/CI etc.”
- [ ] Set branch protection to require PRs, CI, and single PR/issue rules.
- [ ] Ensure all subsequent agent/codex sessions are prompted:  
    > “Read `docs/SESSION_STATE.md` and `AGENTS.md`. Do not begin coding until you acknowledge guardrails and summarize project state from these files.”

---

## 7. FOR AI REPLAY/CODEX UPTAKE

**Replay prompt:**  
> “You are being given the output of a deep Copilot/Copilot Chat research/planning session to seed your session memory and project context for the `dvntone/wscanplus` repo. Start every coding session by reading `docs/SESSION_STATE.md` and `AGENTS.md`, confirm you understand the guardrails, and begin with the next uncompleted step from Phase 0 in `ROADMAP.md`. Output must not violate any shell/safety/secrets/CI/scoping rules contained herein. All other Copilot/Codex agents should begin from this planning session’s context unless explicitly instructed otherwise.”

---

_This document is the live memory and reproducible context for all future researchers, code agents, and contributors to `wscanplus`_.  
_Update as major decisions are made, phases completed, or requirements change._

---

**End of planning session 2026-03-10.**
