# Release and Promotion Standard

**Scope:** All Cajun Craft Software applications  
**Maintained in:** `cajuncraft/software-development-standards`

---

## 1. Purpose

This document defines how code progresses from development through testing to production
in Cajun Craft Software projects. It establishes the approval requirements at each gate
and clarifies what promotion means and does not mean.

---

## 2. Environment Stages

### 2.1 DEV

DEV is for implementation and iteration. DEV runs on the developer's local machine,
uses isolated data, and may be in an incomplete or unstable state.

In DEV:

- Claude implements and tests.
- No real user data is involved.
- The server, if any, is bound to `127.0.0.1` only.
- Features may be partially implemented.
- Failing tests or rough edges are acceptable during active iteration.

### 2.2 TEST

TEST is for controlled validation against a stable build. TEST is not the same as
running DEV on a different machine.

In TEST:

- The build is stable and all tests pass.
- Glen performs manual validation.
- Defects are logged and triaged.
- No PROD data is involved.
- TEST is not publicly accessible.

TEST requires Glen's approval to enter.

### 2.3 PROD

PROD is the live, user-facing environment. PROD uses real data, may be accessible
beyond the developer's machine, and is subject to the full weight of the safety rules.

In PROD:

- Only Glen may authorize promotion.
- All prior gates (DEV, TEST) must have been completed.
- The promotion plan must have been reviewed and approved explicitly.
- Rollback capability must exist and be documented before PROD promotion occurs.

---

## 3. What Promotion Is — and Is Not

### Merging to `main` is not a promotion.

Merging a pull request to the `main` branch means the code is considered production-
ready in the repository. It does not mean any running instance has been updated. It
does not mean PROD is affected.

Promotion is a deliberate runtime act: updating a running server, replacing a packaged
app, or changing an environment variable so that a new environment takes effect. Each
of these requires Glen's explicit approval.

### A recommendation is not an approval.

A PR description that says "ready for PROD" is a recommendation. A Codex verification
that passes all checks is a recommendation. Only Glen's explicit instruction — "yes,
promote" — constitutes approval.

### Having access is not authorization.

An agent that can technically reach a protected action — because it holds the
capability, the credentials, or an allowlisted command — is still not authorized to take
it. Access and ability are not permission. Promotion, merge, packaging, exposure, and
runtime changes are gated by Glen's explicit authorization, not by whether the action is
technically reachable. See the canonical protected-actions list and the definition of
what counts as authorization in the
[Development Operating Model §6 and §6.1](development-operating-model.md).

---

## 4. Approval Gates

| Stage transition | Who approves | Prerequisites |
|---|---|---|
| DEV → PR opened | Claude creates; Codex verifies | Tests pass; PR description complete |
| PR → merge to `main` | Glen (Codex verification required) | Codex review complete; Glen approves |
| `main` → TEST | Glen | Code stable; test environment ready |
| TEST → PROD | Glen | TEST validation complete; rollback documented |
| Any → LAN/network exposure | Glen | Security review; binding change approved |
| Any → packaging/distribution | Glen | Explicit packaging lane approved |
| Any → legacy replacement | Glen | Replacement validated; rollback available |

---

## 5. No-Promotion List

This is the promotion-and-exposure subset of the canonical protected-actions list in the
[Development Operating Model §6](development-operating-model.md); it is restated here for
convenience, not as a separate authority. The following may never occur without Glen's
explicit, per-action approval:

- Binding a server to anything other than `127.0.0.1`.
- Exposing a service via LAN, NAS, Cloudflare, or any network path.
- Replacing the live/legacy version of an app on the user's machine or a shared server.
- Creating a distributable package or installer.
- Changing environment variables on a shared or production machine.
- Starting or restarting a service that affects real user data.

---

## 6. Per-App Promotion Runbooks

Before any app is promoted to real PROD use, that app's repository must document its
own promotion runbook covering:

- The promotion steps (what to run, in what order, on what machine).
- Rollback steps if the promotion fails.
- How to verify the promotion succeeded.
- Who to notify when PROD has been updated.

The promotion runbook must exist in the app repository before the first PROD promotion.
It may not be written during or after the promotion.

---

## 7. Rollback Requirement

Rollback capability is a prerequisite for PROD promotion, not a nice-to-have.

Before any PROD promotion:

- A rollback plan must be written and reviewed.
- The rollback procedure must be tested or walkthrough-verified.
- Any data migration included in the promotion must be reversible, or the
  irreversibility must be explicitly acknowledged and accepted by Glen.

---

## 8. Summary

```
DEV    →  implementation and iteration (Claude owns this lane)
TEST   →  controlled validation (Glen approves entry)
PROD   →  live, real data (Glen is the only gatekeeper)

Merge to main ≠ promotion to PROD
AI recommendation ≠ Glen approval
Mechanical exception ≠ process change
```
