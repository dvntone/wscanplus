# Task Proposals from Codebase Review

## 1) Typo Fix Task
**Issue found:** Status label uses `WONT FIX` (missing apostrophe) in the issue status key.

**Where:** `KNOWN_ISSUES.md`

**Proposed task:**
- Update the status label from `[WONT FIX]` to `[WON'T FIX]` for grammar consistency with the other labels.
- Run a quick docs lint/spell pass on `KNOWN_ISSUES.md` to catch similar wording inconsistencies.

**Acceptance criteria:**
- Status key contains `[WON'T FIX]` and no `WONT FIX` occurrences remain in `KNOWN_ISSUES.md`.

---

## 2) Bug Fix Task
**Issue found:** `POST /api/start-monitoring` destructures `request.body` without guarding for empty/undefined payloads, which can throw before validation.

**Where:** `routes/scan.js`

**Proposed task:**
- Add schema validation for `request.body` (e.g., `techniques` must be a non-empty array of allowed technique strings).
- Add a defensive fallback before destructuring (`const body = request.body || {}`) if schema validation is not added.
- Return `400` with a clear validation message for invalid payloads.

**Acceptance criteria:**
- Empty payload or malformed payload does not crash route handler.
- Route returns deterministic `400` JSON response for invalid payloads.

---

## 3) Code Comment / Documentation Discrepancy Task
**Issue found:** Documentation still describes backend architecture as Express, while the current server implementation is Fastify.

**Where:** `docs/MULTI_PLATFORM_ARCHITECTURE.md` vs `server.js`

**Proposed task:**
- Replace outdated Express references with Fastify in architecture docs.
- Add one canonical architecture reference section that other docs can link to, to reduce drift.
- Include a doc verification check in CI (basic grep guard) to prevent reintroducing stale “Express backend” references for current server runtime docs.

**Acceptance criteria:**
- `docs/MULTI_PLATFORM_ARCHITECTURE.md` and other core setup docs align with Fastify-based backend.
- No stale “Node.js + Express (server.js)” phrasing remains in primary architecture/setup documents.

---

## 4) Test Improvement Task
**Issue found:** Wi-Fi scanner unit tests are fully skipped, reducing confidence in core detection helper behavior.

**Where:** `test/wifi-scanner.test.js`

**Proposed task:**
- Remove `describe.skip` by fixing the UUID/Jest compatibility issue (mock UUID import or isolate UUID usage from pure helper exports).
- Keep helper-level tests as fast unit tests and leave integration behavior in integration suites.
- Add CI coverage gate for this test file to prevent accidental re-skipping.

**Acceptance criteria:**
- `test/wifi-scanner.test.js` runs in CI (not skipped).
- Existing helper assertions pass and coverage includes scanner helper utilities.
