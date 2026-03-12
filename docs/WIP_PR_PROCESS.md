# WIP/Draft PR Review Process

## Problem Solved
Previously, there was no clear mechanism for contributors to signal when a Work-In-Progress (WIP) or DRAFT pull request was ready for review. Reviewers had no way to know whether a PR marked as WIP was actually ready to be reviewed.

## Solution Implemented

### 1. Pull Request Template (`.github/pull_request_template.md`)
Created a comprehensive PR template that includes:
- **WIP/Draft Status Section**: Clear instructions on when to keep a PR in draft and when to mark it ready
- **Ready for Review Checklist**: Explicit criteria that must be met before requesting review:
  - All implementation work complete (no TODOs/placeholders)
  - All tests pass locally
  - CI checks are green
  - All pre-review checklist items checked
- **Pre-Review Checklist**: Incorporates all requirements from `docs/AGENTS.md`
- **For Reviewers Section**: Clear guidance that PRs should not be reviewed until converted from draft

### 2. Updated `docs/AGENTS.md`
Added section **§1.1 WIP / Draft PR Workflow** with:
- **When to use Draft status**: Guidelines for opening PRs as DRAFT
- **Transitioning from Draft to Ready**: 5-step process to convert WIP to ready
- **For Reviewers**: Clear instruction to only review non-draft PRs

### 3. Updated `.github/copilot-instructions.md`
Added rule #6: "Open PRs as DRAFT; mark 'Ready for Review' only when CI is green and all checklist items are complete"

## Usage

### For PR Authors:
1. Open all PRs as **DRAFT** when starting work
2. Keep PR in draft while:
   - Implementation is ongoing
   - Tests are incomplete or failing
   - CI is red
   - Any checklist items are incomplete
3. When ready for review:
   - Ensure CI is green
   - Complete all checklist items in the PR template
   - Convert from draft to "Ready for Review"
   - Request reviewers

### For Reviewers:
- **Do not review DRAFT PRs** — they are not ready
- Only review PRs explicitly marked "Ready for Review"
- If a PR is marked ready but doesn't meet criteria (CI red, incomplete checklist), ask author to convert back to draft

## Benefits
- Clear signal when PRs are ready for review
- Reduces wasted reviewer time on incomplete work
- Enforces quality standards before review requests
- Aligns with project's "Reliability → Traceability → Safety → Velocity" priority
- Maintains the one-PR-at-a-time policy while allowing WIP visibility
