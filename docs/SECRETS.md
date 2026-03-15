# wscan+ Required Secrets

All secrets must be configured in GitHub repository settings before CI/CD can run end-to-end.
**Never commit actual values.** This file lists secret names and purpose only.

---

## Android Signing

| Secret Name | Purpose |
|-------------|---------|
| `ANDROID_KEYSTORE` | Base64-encoded release keystore file (`.jks` or `.keystore`) |
| `ANDROID_KEY_ALIAS` | Alias of the signing key within the keystore |
| `ANDROID_KEY_PASSWORD` | Password for the signing key |
| `ANDROID_STORE_PASSWORD` | Password for the keystore itself |

**Status:** Keystore already created. Secrets not yet configured in GitHub.

To encode keystore for CI:
```bash
# Linux (GNU coreutils base64)
base64 -w 0 release.keystore > keystore.b64

# macOS / BSD base64
base64 -b 0 release.keystore > keystore.b64

# Then paste contents of keystore.b64 as the ANDROID_KEYSTORE secret value
```

---

## Google APIs (Android App)

| Secret Name | Purpose |
|-------------|---------|
| `GOOGLE_MAPS_API_KEY` | Google Maps SDK for Android — scan history map, network heatmap |
| `VERTEX_AI_API_KEY` | Vertex AI / Gemini — in-app threat analysis and AI heuristics |

**Status:** Both APIs activated on Google Cloud (Google AI Pro). Secrets not yet configured in GitHub.

---

## How to Configure

1. Go to **github.com/dvntone/wscanplus → Settings → Secrets and variables → Actions**
2. Click **New repository secret**
3. Add each secret by name with its value
4. Secrets are available in workflows as `${{ secrets.SECRET_NAME }}`

---

## Local Development

For local Android builds, add to `android/local.properties` (git-ignored):
```
GOOGLE_MAPS_API_KEY=your_key_here
VERTEX_AI_API_KEY=your_key_here
ANDROID_KEY_ALIAS=your_alias
ANDROID_KEY_PASSWORD=your_password
ANDROID_STORE_PASSWORD=your_password
```

For local desktop builds, add to `.env` (git-ignored):
```
# See .env.example for all required variables
```

---

*Never commit `local.properties`, `.env`, keystore files, or any file containing actual secret values.*
