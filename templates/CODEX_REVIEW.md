# Codex Review Checklist — [APP NAME]

> **Template:** Copy this file to the root of a new Cajun Craft Software app repo.
> Replace all `[PLACEHOLDER]` values with app-specific details.
> Codex fills this checklist for every PR before recommending merge to Glen.

---

## Purpose

This checklist defines what Codex must verify before recommending a merge. Codex
reviews independently — without relying on Claude's summary as a substitute for
running the tests.

Codex is the verification intelligence of the operating model, not a passive
box-checker. Codex is expected to actively challenge:

- Weak test coverage (a test suite that exists but provides little meaningful
  confidence is not a pass).
- Technically fragile or risky approaches, even when tests pass.
- Scope drift — files changed outside the approved lane.
- Shallow PR descriptions that do not reflect the actual diff.
- Poor implementation quality that would create downstream problems.

A completed checklist is a recommendation only. Glen approves the merge.

**Note:** Apps with real deployment pipelines (NAS, Docker, server promotion) may
also need a separate Codex OPS document defining the operational execution lane.
This checklist covers verification only.

---

## Pre-Review Requirements

Before beginning, Codex must:

- [ ] Check out the PR branch locally.
- [ ] Confirm the branch targets `main` (not another feature branch).
- [ ] Run the full test suite independently. Do not rely on Claude's test output.
- [ ] Review the full diff, not just the summary.

---

## Scope Verification

- [ ] The PR description states the approved lane or work order.
- [ ] Changed files are consistent with the stated scope.
- [ ] No files outside the approved scope were modified.
- [ ] No source code was changed when this is a docs-only PR (and vice versa).
- [ ] The lane summary (`docs/ai-handoff/<lane>-summary.md`) was updated.
- [ ] `docs/ai-handoff/current.md` was updated if the active lane changed.

## Test Results

- [ ] All existing tests pass with no skips or unexplained failures.
- [ ] New tests cover the new behavior described in the PR.
- [ ] Test coverage is sufficient for the risk level of the change.
- [ ] No tests were deleted or weakened without a stated reason.

## Code Review

- [ ] Changed code is consistent with the repo's existing style and conventions.
- [ ] No hardcoded secrets, credentials, passwords, or tokens.
- [ ] No new external dependencies introduced without Glen's approval.
- [ ] No binding to anything other than `127.0.0.1` (for local web apps).
- [ ] No files committed that should be in `.gitignore` (venvs, build artifacts, etc.).
- [ ] [APP-SPECIFIC checks, e.g., "No changes to the legacy baseline app"]

## Safety

- [ ] No production data paths are touched.
- [ ] No Cloudflare, DNS, or network configuration is changed.
- [ ] No promotion or deployment scripts were triggered or modified to auto-execute.
- [ ] Rollback is possible: the previous behavior can be restored by reverting the PR.

## Documentation

- [ ] PR description accurately describes what changed.
- [ ] Any new behavior is documented (in code comments, docstrings, or docs/).
- [ ] The handoff file Next Actions are plausible for the next session.

---

## Verification Result

**Date reviewed:**  
**Branch:**  
**Commit SHA:**  
**Test command run:**  
**Test result:**  

**Summary of findings:**

*(List any concerns, out-of-scope files, weak tests, or other issues found.)*

**Recommendation:**

- [ ] Recommend merge — all checks passed; no findings.
- [ ] Recommend merge with notes — minor findings noted above; not blocking.
- [ ] Do NOT recommend merge — blocking issue(s) noted above.

**Merge may only proceed after Glen's explicit approval.**
