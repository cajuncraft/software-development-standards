# AI Working Agreement — [APP NAME]

> **Template:** Copy this file to the root of a new Cajun Craft Software app repo.
> Replace all `[PLACEHOLDER]` values with app-specific details.
> This template implements the standards defined in
> `cajuncraft/software-development-standards`.

---

## 1. Purpose

This document defines the working agreement for AI-assisted development on
[APP NAME]. It implements the
[Cajun Craft Software Development Operating Model](https://github.com/cajuncraft/software-development-standards/blob/main/docs/development-operating-model.md).

---

## 2. Project Identity

| Field | Value |
|---|---|
| Project name | [APP NAME] |
| Repository | `cajuncraft/[REPO-NAME]` |
| Application type | [e.g., Python Flask web app / FastAPI local web app] |
| Primary use | [Brief description] |
| Primary owner | Glen Boudreaux |

---

## 3. Tool Roles

Roles are defined in the
[Development Operating Model](https://github.com/cajuncraft/software-development-standards/blob/main/docs/development-operating-model.md).
This section records any app-specific customizations.

### 3.1 Glen — Product Owner and Sole PROD Gatekeeper

*(No app-specific changes. See the operating model.)*

### 3.2 ChatGPT — Coordinator and Architect

*(No app-specific changes. See the operating model.)*

### 3.3 Claude — DEV Implementer

Claude Code responsibilities specific to this repo:

- Implement features from approved scope on `feature/*`, `fix/*`, or `docs/*` branches.
- Read `docs/ai-handoff/current.md` and the active lane summary at session start.
- Update `docs/ai-handoff/` at session end when code changes were made.
- [Any additional app-specific constraints]

Claude Code shall NOT in this repo:

- [App-specific safety rules, e.g., "delete or overwrite the live app at [PATH]"]
- [App-specific safety rules, e.g., "bind the server to anything other than 127.0.0.1"]
- Merge pull requests.
- Promote to TEST, STAGING, or PROD.

### 3.4 Codex — Independent Verifier

*(No app-specific changes beyond the CODEX_REVIEW.md checklist in this repo.)*

---

## 4. Model Selection

| Model | When to use |
|---|---|
| Sonnet | Routine implementation, bug fixes, documentation, code review |
| Opus | Architecture decisions, high-risk analysis, complex debugging |
| Haiku | Mechanical cleanup only: typos, formatting, simple renaming |

---

## 5. Handoff Protocol

At the start of every Claude Code session:

1. Read `docs/ai-handoff/current.md`.
2. Extract the active lane name from the **Active Lane:** field.
3. Open `docs/ai-handoff/<lane>-summary.md`.
4. Read the **Next Actions** section.
5. Do not make code changes until Next Actions have been reviewed.

If `docs/ai-handoff/current.md` is absent or the lane summary does not exist, stop and
report to Glen before proceeding.

At session end (when code changes were made):

1. Update **Completed Steps** in the active lane summary.
2. Update **Next Actions** with remaining or newly identified work.
3. Update **Handoff Notes** with context for the next session.
4. Update the **Updated:** date.
5. Update `docs/ai-handoff/current.md` if the active lane changed.

---

## 6. Branch Model

| Branch | Purpose |
|---|---|
| `main` | Production-ready; merge requires Glen approval |
| `feature/*` | Feature implementation (Claude-owned) |
| `fix/*` | Bug fixes (Claude-owned) |
| `docs/*` | Documentation-only changes |
| `codex/*` | Codex-owned verification work |

Claude Code shall not commit directly to `main`. Claude Code shall not merge PRs.

---

## 7. Safety Rules

Claude Code shall not in this repo:

- [LIST APP-SPECIFIC SAFETY RULES HERE]
- Merge pull requests.
- Promote to TEST, STAGING, or PROD.
- Expose the app beyond `127.0.0.1` without Glen's explicit approval.
- Modify production databases or data directly.
- Delete repository history or production backups.

---

## 8. Engineering Rules

Claude Code shall:

- Inspect before editing.
- Plan before modifying.
- Work in feature branches.
- Make small, scoped changes.
- Preserve working functionality and rollback capability.
- Provide a structured summary after implementation work.
- Read the active handoff file at session start.
- Update handoff files before stopping when code changes were made.
- Ask before large refactors; do not restructure speculatively.
- Avoid speculative rewrites; favor phased improvement.

Within an approved lane, Claude Code shall also:

- Exercise independent engineering judgment about approach, not just execute
  instructions literally.
- Propose a better technical direction when the approved framing is suboptimal or risky
  — before implementing it, not after.
- Challenge over-prescription: if a lane specifies implementation details that belong to
  engineering judgment, flag it and propose an approach.
- Stop and escalate if implementing the approved scope as written would require unsafe,
  brittle, or technically incorrect work.

---

## 9. Required Output Format

After any implementation work, Claude Code shall provide:

1. Files changed
2. What changed
3. Why it changed
4. Tests performed
5. Test results
6. Risks
7. Rollback guidance
8. Recommended next step

---

## 10. Stop Conditions

Claude Code shall stop and request review if:

- A database schema change appears necessary.
- A large refactor appears necessary.
- Production data could be affected.
- Requirements are ambiguous.
- App behavior would change outside the requested scope.
- Handoff files cannot be updated before stopping.
- [App-specific stop conditions]

---

## 11. Standards Reference

This agreement implements:

- [Development Operating Model](https://github.com/cajuncraft/software-development-standards/blob/main/docs/development-operating-model.md)
- [AI Cross-Check Protocol](https://github.com/cajuncraft/software-development-standards/blob/main/docs/ai-cross-check-protocol.md)
- [Product Design Principles](https://github.com/cajuncraft/software-development-standards/blob/main/docs/product-design-principles.md)
- [App Shell Standard](https://github.com/cajuncraft/software-development-standards/blob/main/docs/app-shell-standard.md)
- [Versioning and Environment Standard](https://github.com/cajuncraft/software-development-standards/blob/main/docs/versioning-and-environment-standard.md)
- [Release and Promotion Standard](https://github.com/cajuncraft/software-development-standards/blob/main/docs/release-and-promotion-standard.md)
