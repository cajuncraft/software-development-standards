# Development Operating Model

**Scope:** All Cajun Craft Software projects  
**Maintained in:** `cajuncraft/software-development-standards`

---

## 1. Purpose

This document defines how Glen, ChatGPT, Claude, and Codex collaborate on Cajun Craft
Software projects. The model is designed to preserve ownership clarity, prevent
unauthorized changes, and keep Glen in control of every production gate.

---

## 2. Core Model

```
Glen + ChatGPT  →  define product intent and scope
Claude          →  reviews and proposes development direction
Glen + ChatGPT  →  approve scope
Claude          →  owns DEV proposal, implementation, tests, and PR creation
Codex           →  independently verifies scope, tests, security, and merge readiness
Glen            →  approves or rejects merge; approves all promotions
```

No step in this chain may be skipped. Verbal summaries and AI recommendations are not
approvals. Only Glen's explicit approval is an approval.

---

## 3. Role Definitions

### 3.1 Glen — Product Owner and Sole PROD Gatekeeper

Glen is the final decision maker for all Cajun Craft Software projects.

Glen:

- Defines and approves product intent and scope.
- Approves or rejects promotions at every pipeline gate (DEV → TEST → PROD).
- Is the only person authorized to approve merges to `main` and promotions to PROD.
- Performs TEST validation and logs defects.
- May delegate scoped mechanical tasks to Codex with explicit, bounded approval.

A recommendation from any AI tool is not an approval. Only Glen's explicit instruction
counts.

### 3.2 ChatGPT — Coordinator and Architect

ChatGPT coordinates scope, architecture, and workflow intent across projects.

ChatGPT:

- Defines and clarifies product scope in collaboration with Glen.
- Reviews architecture proposals and implementation plans.
- Issues recommendations to Glen (a recommendation is not an approval).
- Acts as integration control plane across multiple AI actors.
- Interprets tester feedback and implementation reports.

ChatGPT does not implement code, does not open PRs, and does not approve merges.

### 3.3 Claude — DEV Implementer

Claude owns DEV implementation. Claude does not merge and does not promote.

Claude:

- Implements features from approved scope on `feature/*`, `fix/*`, or `docs/*` branches.
- Proposes development direction; does not begin implementation until scope is approved.
- Writes and updates tests as part of every implementation lane.
- Opens pull requests and writes PR descriptions.
- Reads and updates `docs/ai-handoff/` at session start and end.
- Reports implementation results in a structured format.

Claude shall NOT:

- Merge pull requests.
- Promote to TEST, STAGING, or PROD.
- Expand scope beyond the approved lane.
- Push directly to `main`.
- Skip tests or documentation steps.
- Treat a mechanical exception as a process change.

### 3.4 Codex — Independent Verifier

Codex independently verifies Claude's work and may merge only with Glen's explicit approval.

Codex:

- Reviews Claude's implementation in isolation, without access to Claude's reasoning.
- Runs tests and reviews changed files, security impact, and scope adherence.
- Documents verification results before recommending a merge.
- Performs small scoped tasks assigned explicitly by Glen (using `codex/*` branches).

Codex shall NOT:

- Merge without Glen's explicit, per-merge approval.
- Promote to TEST, STAGING, or PROD without Glen's approval.
- Edit Claude-owned branches without Glen's approval.
- Work directly in `main`.
- Accept a weak or incomplete test run as a pass.

### 3.5 Gemini (Optional) — Architecture Reviewer

Gemini may be used as an independent architecture reviewer or second-opinion tool at
Glen's discretion. Gemini has no implementation or merge authority.

---

## 4. Approval Gates

| Action | Who approves |
|---|---|
| Scope of a lane | Glen (with ChatGPT recommendation) |
| Implementation plan | Glen (with ChatGPT second opinion) |
| Merge to `main` | Glen (Codex verification required first) |
| Promotion to TEST | Glen |
| Promotion to STAGING | Glen |
| Promotion to PROD | Glen |
| LAN / NAS / Cloudflare exposure | Glen |
| Packaging / runtime service start | Glen |
| Legacy app replacement | Glen |
| Mechanical exception | Glen (the exception is not a process change) |

---

## 5. What Is a Mechanical Exception

A mechanical exception is a bounded, one-time deviation from process — for example,
fixing a typo in a commit message, resolving a merge conflict after Glen's review, or
re-running a failing test that had a transient infrastructure error.

A mechanical exception:

- Applies to exactly the approved action. Nothing more.
- Does not create precedent.
- Does not change the process.
- Must be reported to Glen.

If any AI actor treats a mechanical exception as permission to act differently in future
situations, that is a process violation.

---

## 6. No-Authorization List

The following actions may never occur without Glen's explicit approval:

- Merging a pull request.
- Promoting to TEST, STAGING, or PROD.
- Packaging or creating a distributable artifact.
- Starting, restarting, or binding a production runtime service.
- Exposing any service via LAN, NAS, Cloudflare, port forwarding, or any other network
  path beyond 127.0.0.1.
- Replacing or removing the legacy/baseline version of an app.
- Deleting repository history, branches, or production backups.
- Modifying DNS, Cloudflare, or infrastructure configuration.

---

## 7. Working Philosophy

> Preserve what works.  
> Improve in phases.  
> Automate safely.  
> Document decisions.  
> Keep rollback simple.  
> Use AI as an accelerator, not an uncontrolled operator.
