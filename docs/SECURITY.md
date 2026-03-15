# wscan+ Security Policy

Security requirements that must be in place before any code ships. These apply to all agents and contributors.

---

## Process Rules

- **No new feature work while a `blocker` or `security`-labeled issue is open.**
- Every caught exception must be logged — never silently swallowed.
- Validate all user input and all arguments passed to child processes.
- No `exec()` with interpolated strings. Use `spawn(cmd, [args])` only.

---

## Secrets Management

- Never commit secrets. See `docs/SECRETS.md` for the full list of required secrets.
- Android: `local.properties` + secrets-gradle-plugin (git-ignored).
- Desktop: `.env` + dotenv (git-ignored). See `.env.example` for required variables.
- Verify `.env` and `local.properties` are not tracked before every PR.

---

## Desktop App (Electron / Node.js)

### HTTP Security Headers

All HTTP responses from the local desktop server must include:

| Header | Required Value |
|--------|---------------|
| `Content-Security-Policy` | `default-src 'self'; script-src 'self'; style-src 'self' 'unsafe-inline'` |
| `X-Content-Type-Options` | `nosniff` |
| `X-Frame-Options` | `DENY` |
| `Strict-Transport-Security` | `max-age=31536000; includeSubDomains` (HTTPS only) |
| `Referrer-Policy` | `no-referrer` |

### CORS

- Controlled via `ALLOWED_ORIGINS` environment variable (see `.env.example`).
- Default: `localhost` only. Never `*` in production.
- Validate `Origin` header on every request.

### Rate Limiting

- All API endpoints must have rate limiting configured before any code ships.
- Minimum: 60 requests/minute per IP for general endpoints.
- Stricter limits on auth or sensitive endpoints.

### Child Process Safety

```js
// CORRECT — always use spawn with argument array
import { spawn } from 'node:child_process';
spawn('iw', ['dev']);

// NEVER DO THIS
exec(`iw ${userInput}`);  // shell injection risk
```

### Electron-Specific

- Never run the Electron app as root.
- Set `nodeIntegration: false` and `contextIsolation: true` in all BrowserWindow instances.
- Use `contextBridge` to expose only required APIs to renderer.

---

## Android App

- All network communication over HTTPS only (no HTTP cleartext).
- Use Android Keystore for any locally stored keys or credentials.
- Request only the minimum permissions required for each feature.
- Background scanning must have a visible persistent notification.
- No data leaves the device without explicit user action (export, share, sync).

---

## CI Secret Scanning

The CI workflow runs a secret scan on every PR. A match causes the build to **fail** — do not bypass without understanding the match.

The scanner looks for common high-risk secret formats, including (but not limited to):
- AWS access key IDs (for example, patterns starting with `AKIA` such as `AKIA[0-9A-Z]{16}`)
- Google API keys (for example, keys starting with `AIza` / `AIzaSy`)
- GitHub personal access tokens (for example, prefixes like `ghp_` / `ghs_`)
- PEM/PGP blocks (for example, lines starting with `-----BEGIN`)

The exact patterns are defined in `.github/workflows/ci.yml`. Refer to that file when triaging secret-scan failures or suspected false positives.

If a false positive is triggered, document the reason in the PR description and request maintainer review.
