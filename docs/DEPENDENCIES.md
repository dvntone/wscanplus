# wscan+ Dependencies (Pinned)

## Desktop Dev Environment (Linux)
Required:
- Node.js: 20.x (use .nvmrc when added)
- npm: 10.x
- Git: 2.40+
- adb: android-tools-adb (verify: `adb version`)
- JDK 17 (Temurin recommended)

Recommended for advanced features:
- iw (verify: `iw --version` or `iw dev`)
- aircrack-ng 1.7+ (verify: `airmon-ng --help`)
- tshark 4.0+ (verify: `tshark --version`)
- nmcli (verify: `nmcli --version`)

## Android Dev Environment
- Android Studio (current stable)
- compileSdk/targetSdk pinned in Gradle (do not hardcode in CI)
- JDK 17
- Gradle via wrapper (`./gradlew`)

## Verification commands (run before coding)
- Node: `node --version && npm --version`
- Android: `cd android-native && ./gradlew -v`
- ADB: `adb devices`
