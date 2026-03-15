# wscan+ Dependencies (Pinned)

## Dependency Pinning Policy

- **npm**: exact versions only — no `^` or `~` in `package.json`. Use `npm install --save-exact`.
- **Android Gradle**: pin all dependency versions explicitly. No dynamic versions (`+`).
- When adding a dependency, create a dedicated dependency-only PR per docs/AGENTS.md §3.

---

## Desktop Dev Environment (Linux)

### Required

| Tool | Version | Verify |
|------|---------|--------|
| Node.js | **22.x** (see `.nvmrc`) | `node --version` |
| npm | 10.x | `npm --version` |
| Git | 2.40+ | `git --version` |
| adb (android-tools-adb) | 35.0+ | `adb version` |
| JDK | 17 (Temurin recommended) | `java -version` |

### Optional (advanced features)

| Tool | Min Version | Verify | Purpose |
|------|------------|--------|---------|
| iw | any | `iw --version` | Adapter detection (`iw dev`) |
| aircrack-ng | 1.7+ | `airmon-ng --help` | Monitor mode, injection testing |
| tshark | 4.0+ | `tshark --version` | PCAP analysis pipeline |
| nmcli | any | `nmcli --version` | Network interface management |
| Kismet | 2023-07+ | `kismet --version` | Optional scan integration |
| BetterCap | 2.32+ | `bettercap --version` | Optional scan integration |
| Wireshark | 4.0+ | `wireshark --version` | PCAP analysis (GUI) |

---

## Android Dev Environment

| Tool | Version | Notes |
|------|---------|-------|
| Android Studio | current stable | |
| JDK | 17 | Same as desktop |
| Gradle | via wrapper (`./gradlew`) | Do not install globally |
| compileSdk / targetSdk | pinned in Gradle | Do not hardcode in CI |
| Android platform-tools | 35.0+ | Must match desktop adb version |

---

## Verification Commands (run before starting work)

```bash
# Desktop
node --version && npm --version
git --version
adb version
java -version

# Optional tools
if command -v iw >/dev/null 2>&1; then iw --version 2>/dev/null; else echo "iw not installed"; fi
if command -v airmon-ng >/dev/null 2>&1; then airmon-ng --help 2>/dev/null | head -1; else echo "airmon-ng not installed"; fi
if command -v tshark >/dev/null 2>&1; then tshark --version 2>/dev/null | head -1; else echo "tshark not installed"; fi
if command -v kismet >/dev/null 2>&1; then kismet --version 2>/dev/null | head -1; else echo "kismet not installed"; fi
if command -v bettercap >/dev/null 2>&1; then bettercap --version 2>/dev/null | head -1; else echo "bettercap not installed"; fi

# Android
cd android && ./gradlew -v
adb devices
```
