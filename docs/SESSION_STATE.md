# SESSION_STATE (source of truth)

Repo: https://github.com/dvntone/wscanplus
Name: wscan+ (WiFi Scan + Companion)
Audience: professional / advanced users (defensive detection & assessment)

---

## Core Architecture

- **Android** (Kotlin, API 31+): GPS companion, field sensor, tethering provider, distributed scanner node
- **Desktop** (Linux-first, Electron/Node): primary hub — scan aggregation, Kismet/BetterCap integration, PCAP analysis, AI heuristics
- **Web UI**: PWA dashboard served locally by desktop — not standalone for scanning

---

## Key Decisions (locked)

- Drop WiGLE service. Mapping via Google Maps (Android) + optional web map.
- Export formats: PCAP/PCAPNG (Wireshark-compatible) + JSON.
- Kismet: OPTIONAL integration. Primary near-term use: Android as remote GPS source.
- BetterCap: OPTIONAL integration. Reference: https://github.com/bettercap/caplets
- Minimal CI only. Do NOT add CodeQL/SonarCloud/Semgrep/Detekt by default.
- Secrets: NO tokens in repo. Android uses `local.properties` + secrets-gradle-plugin; desktop uses `.env` + dotenv.
- AI guardrails: 1 PR at a time, 1 issue per PR, tests-first, stop if CI red.
- **Node.js module system: ESM only.** All `package.json` files must have `"type": "module"`. No CommonJS.
- **Merge strategy: Squash only.** Merge commits and rebase are disabled.

---

## Tethering / Monitor Mode Model

- `airmon-ng` disables normal networking on the scan adapter.
- Android device tethers (USB or hotspot) to provide internet to desktop while adapter is in monitor mode.
- App must detect adapter count via `iw dev` and prompt user:
  - 1 adapter: "Connect a second adapter or tether from Android to maintain connectivity"
  - Android tethering detected: confirm which interface is scan vs. network

---

## ADB Communication Boundary

**Android → Desktop:** GPS position, scan metadata, threat events, tether status, device ID
**Desktop → Android:** commands, alerts, config updates, sync triggers

### Sync Modes (user-configurable, default is user-set)

| Mode | Description |
|------|-------------|
| Store + sync | Store on Android locally → sync when USB/LAN reconnected (default, offline-safe) |
| VPN tunnel | Push through reverse tunnel to desktop in real time |
| Standalone | Android runs independently; desktop is post-session review hub |

### Multi-Device

- Multiple Android devices can act as distributed field sensors simultaneously.
- Each device identified by unique device ID.
- Desktop aggregates and correlates data from all connected clients.
- Data model must support multi-device from day 1.

### ADB Connection Priority

1. USB ADB (most reliable, used when tethered)
2. Wireless ADB (same LAN)
3. User-configured remote sync

---

## Folder Structure

```
wscanplus/
├── android/           # Kotlin companion app (Gradle)
├── desktop/           # Electron app (Linux-first, ESM)
│   ├── src/
│   │   ├── main/      # Electron main process
│   │   └── renderer/  # UI — terminal/advanced + accessible modes
│   └── package.json   # "type": "module"
├── docs/              # All documentation
└── .github/
    └── workflows/     # CI only — no deploy/merge/trigger jobs
```

---

## Phase

**Phase 0 (Foundation / Safety / Build Integrity)** — nearing completion

---

## Phase 0 Remaining Actions

1. ~~Add guardrails: AGENTS.md + .github/copilot-instructions.md~~ ✅
2. ~~Add docs: INDEX, ROADMAP, DEPENDENCIES, SESSION_STATE~~ ✅
3. ~~Add secrets scaffolding: .env.example + secrets.defaults.properties + .gitignore~~ ✅
4. Fix and complete CI: timeouts + Android unit test + blocking secret scan ← **in progress**
5. Configure GitHub repo secrets (GOOGLE_MAPS_API_KEY, VERTEX_AI_API_KEY, Android keystore) ← **pending**
6. Create GitHub labels/milestones for Phase 1 planning ← **pending**

---

## Non-goals (for now)

- No offensive features by default (deauth injection, cracking, etc.)
- No Windows support until Linux path is stable
- No Flatpak/Snap packaging (sandbox blocks required capabilities)
- No tiered/paid licensing in initial build
