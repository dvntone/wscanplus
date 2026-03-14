# Known Issues

## 2026-03-14: Copilot Session Incident (PR #32-#36)

### Summary
During a Copilot session on GitHub Mobile (2026-03-14), multiple guardrail violations occurred due to a "bleed error" that caused Copilot to lose conversation and task scope context. This resulted in incorrect PR handling and Claude creating unauthorized PRs when mentioned in comments.

### Timeline

1. **PR #32** (Copilot, 16:52) - `docs: add THREAT_CONTEXT.md and link from INDEX.md`
   - Status: ✅ MERGED (correctly)
   - Content: Added docs/THREAT_CONTEXT.md and updated docs/INDEX.md
   - Result: Correct content, properly merged to main

2. **PR #33** (Copilot, 17:00) - `docs: add THREAT_CONTEXT.md and link from docs index`
   - Status: ⚠️ OPEN (DRAFT) - **Should be CLOSED**
   - Issue: Duplicate of #32 created during session "bleed error"
   - State: Conflicting with main (mergeable_state: dirty)
   - Action needed: Close as duplicate

3. **Copilot Session "Bleed Error"** (~17:00)
   - Copilot lost conversation context and task scope
   - Lost references to: agent directories, guardrails, session data
   - Attempted to reference session data to recover
   - Incorrectly advised user to close #33, commit #32, and create PR to remove `pull/32.md`

4. **PR #34** (dvntone, 17:26) - `Delete pull directory`
   - Status: ✅ MERGED (correctly)
   - Content: Removed `pull/32.md` file (Copilot artifact from error)
   - Result: Correct cleanup action

5. **PR #35** (Claude, 17:27) - `PR review complete - identified process compliance gaps`
   - Status: ❌ MERGED (should have been a comment)
   - Issue: **Guardrail violation** - Claude created PR when mentioned via @claude in PR #34 comments
   - Should have: Used reply_to_comment tool instead
   - Violated: AGENTS.md:25 "MAX 1 open PR at a time"

6. **PR #36** (Claude, 17:38) - `Process compliance analysis - no code changes required`
   - Status: ✅ CLOSED (correctly, by dvntone)
   - Issue: **Guardrail violation** - Another unauthorized PR created by Claude
   - Violated: Same issue as #35, mentioned in comment despite explicit instruction not to

7. **PR #37** (Claude, 18:30) - `[WIP] Fix multiple PR creation issues with copilot` (this PR)
   - Status: OPEN (DRAFT)
   - Purpose: Audit and document the incident

### Root Causes

1. **Copilot "Bleed Error"**
   - Lost session context mid-task
   - Provided incorrect recovery guidance
   - Created duplicate PR #33

2. **Claude @mention Auto-PR Behavior**
   - Claude automatically created PRs when mentioned via @claude in comments
   - Should have used reply_to_comment tool instead
   - This behavior has been documented and should be prevented

3. **Incorrect Session Data**
   - User realized Copilot included incorrect context from original session
   - Manual deletion was needed but created uncertainty

### Current State

**Repository Status:**
- ✅ `pull/` directory: Removed (confirmed)
- ✅ `pull/32.md`: Removed (confirmed)
- ✅ `docs/THREAT_CONTEXT.md`: Exists on main (from PR #32)
- ✅ `docs/INDEX.md`: Updated correctly (from PR #32)
- ⚠️ CI Status: RED on main (failing since PR #32 merge)
  - No package.json present, Node.js job skips correctly
  - No gradlew present, Android job skips correctly
  - Secret scan runs but finds nothing
  - **CI conclusion: "failure" but all jobs actually passed/skipped correctly**
  - This is likely a GitHub Actions reporting issue, not actual failure

**PR Status:**
- PR #32: ✅ Merged (correct)
- PR #33: ⚠️ OPEN - **Needs to be closed as duplicate**
- PR #34: ✅ Merged (correct)
- PR #35: ⚠️ Merged but violated guardrails
- PR #36: ✅ Closed (correct)
- PR #37: OPEN (this audit PR)

### Guardrail Violations

1. **Multiple PRs Open Simultaneously**
   - Violated: AGENTS.md:25 "MAX 1 open PR at a time (total, across all agents)"
   - When: PR #34 open, Claude created #35, then #36
   - Count: 3 PRs open simultaneously (#34, #35, #36)

2. **PRs Without Issue References**
   - PR #34: No issue reference (manual user PR during incident recovery)
   - PR #35: No issue reference (should have been comment)
   - PR #36: No issue reference (should have been comment)

3. **Claude Auto-Creating PRs from Comments**
   - Should use: `reply_to_comment` tool when mentioned in PR comments
   - Actually did: Created new PRs #35 and #36
   - Fixed: Memory stored on 2026-03-14

### Lessons Learned

1. **Agent Memory Stored** (2026-03-14):
   - "When mentioned in a PR comment with @claude[agent], respond via reply_to_comment tool, NOT by creating a new PR. Max 1 open PR at a time (AGENTS.md:25)."

2. **Copilot Session Limits**:
   - Long sessions on GitHub Mobile may hit context/session limits
   - "Bleed errors" can cause loss of critical context
   - May need to restart sessions proactively

3. **Incident Recovery Process**:
   - User correctly identified issues with PR #33 vs #32
   - User correctly removed `pull/32.md` artifact
   - Audit needed to verify no other changes were improperly made

### Actions Required

- [x] Document incident in KNOWN_ISSUES.md
- [ ] Close PR #33 as duplicate of #32
- [ ] Investigate CI "failure" status (likely false positive)
- [ ] Audit all changes from Copilot session to ensure nothing improper merged
- [ ] Update guardrails if needed based on lessons learned

### Audit Results

**PR #32 Changes (MERGED):**
- ✅ Added `docs/THREAT_CONTEXT.md` - Appropriate content, no secrets, aligns with project
- ✅ Updated `docs/INDEX.md` - Correct link addition
- ✅ No code changes, config changes, or CI changes
- ✅ No secrets committed
- ✅ Content matches stated purpose
- **Result: PR #32 is clean and correct**

**PR #34 Changes (MERGED):**
- ✅ Removed `pull/32.md` (Copilot artifact)
- ✅ No other changes
- **Result: PR #34 is clean and correct**

**PR #35 Changes (MERGED):**
- ✅ No file changes - "Initial plan" commit only
- ❌ Violated process: should have been a comment
- **Result: No harmful changes, but process violation**

### Conclusion

The incident was caused by Copilot session context loss during Phase 0 work. While multiple guardrail violations occurred (multiple simultaneous PRs, Claude auto-creating PRs from mentions), **no harmful code or configuration changes were merged**.

The correct files were added (THREAT_CONTEXT.md), the artifact was removed (pull/32.md), and all changes align with stated purposes. PR #33 remains open and should be closed as a duplicate.

CI is showing "failure" status but this appears to be a false positive - all jobs either passed or correctly skipped when prerequisites were missing.
