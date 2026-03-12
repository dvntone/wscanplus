# SESSION_STATE (source of truth)

Repo: https://github.com/dvntone/wscanplus
Name: wscan+ (WiFi Scan + Companion)
Audience: professional / advanced users (defensive detection & assessment)

## Core Architecture
- Android (Kotlin, API 31+): standalone scanner + companion sensor
- Desktop (Linux-first): Electron/Node hub + optional advanced capture
- Web UI: PWA dashboard (served locally by desktop), not standalone for scanning

## Key Decisions (locked)
- Drop WiGLE (not needed). Mapping via Google Maps (Android) + optional web map.
- Keep export formats that professionals use: PCAP/PCAPNG (Wireshark), JSON.
- Kismet: keep as OPTIONAL integration; primary near-term use is Android-as-remote-GPS source.
- Minimal CI only. Do NOT add CodeQL/SonarCloud/Semgrep/Detekt by default.
- Secrets: NO tokens in repo. Android uses local.properties + secrets-gradle-plugin; desktop uses .env + dotenv.
- AI guardrails: 1 PR at a time, 1 issue per PR, tests-first, stop if CI red.

## Phase
Phase 0 (Foundation / Safety / Build Integrity)

## Next Actions (in order)
1. Add docs: docs/INDEX.md, docs/ROADMAP.md, docs/DEPENDENCIES.md
2. Add secrets scaffolding: .env.example + secrets.defaults.properties + strict .gitignore
3. Simplify / fix CI: build+test+lint + secret scan + timeouts
4. Setup GitHub labels/milestones/project board; create Phase 0 issues

## Non-goals (for now)
- No offensive features by default (deauth injection, cracking, etc.)
- No Windows support until Linux path is stable
- No Flatpak/Snap packaging (sandbox blocks required capabilities)
