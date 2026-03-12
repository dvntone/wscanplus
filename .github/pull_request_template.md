# Pull Request

## Issue Reference
Closes #<issue_number>

## Description
<!-- Brief description of what this PR does -->

## Type of Change
- [ ] Bug fix (non-breaking change which fixes an issue)
- [ ] New feature (non-breaking change which adds functionality)
- [ ] Dependency update (adds or updates dependencies)
- [ ] Maintenance (refactoring, cleanup, documentation)
- [ ] Breaking change (fix or feature that would cause existing functionality to not work as expected)

## WIP/Draft Status
<!-- If this PR is marked as DRAFT/WIP, please:
     1. Keep it in draft status until ALL checklist items below are complete
     2. Mark "Ready for Review" only when CI is green and all items are checked
     3. Tag reviewers ONLY when converting from draft to ready
-->

**This PR is ready for review when:**
- [ ] All implementation work is complete (no TODOs or placeholders)
- [ ] All tests pass locally
- [ ] CI checks are green
- [ ] All items in the "Pre-Review Checklist" below are checked

---

## Pre-Review Checklist

### Requirements (from docs/AGENTS.md)
- [ ] References exactly one GitHub Issue (linked above)
- [ ] CI is green before marking ready for review
- [ ] One feature OR one bugfix (not both)
- [ ] < 200 LOC changed (tests excluded)
- [ ] No new dependencies mixed in (or this IS a dependency-only PR)
- [ ] Meaningful tests included/updated (no placeholder assertions)
- [ ] Errors are logged, not silently swallowed
- [ ] Documentation updated (or doc issue filed)
- [ ] `.env` / `local.properties` NOT committed
- [ ] No secrets of any kind committed
- [ ] Required platform checks passed (see below)

### Platform-Specific Checks
**If this PR touches Android code:**
- [ ] `./gradlew :core:test` passes
- [ ] Assemble debug/release builds successfully (as applicable)

**If this PR touches Node/Desktop code:**
- [ ] `npm test` passes
- [ ] `npm run lint` passes

### Security & Safety
- [ ] No `exec()` with interpolated strings (use `spawn(cmd, [args])` only)
- [ ] All subprocess arguments are validated (paths, ports, interfaces)
- [ ] No secrets or credentials in code or config files

---

## Testing Performed
<!-- Describe the testing you did to verify your changes work correctly -->

- [ ] Unit tests added/updated
- [ ] Manual testing performed
- [ ] Edge cases considered and tested

**Test commands run:**
```bash
# List the commands you ran to test your changes
```

---

## Additional Notes
<!-- Any additional context, screenshots, or information for reviewers -->

---

## For Reviewers
**Do not approve or merge this PR unless:**
1. PR is no longer in DRAFT status
2. All checklist items above are checked
3. CI is green
4. Code review feedback is addressed
