# CLAUDE.md — [APP NAME]

> **Template:** Copy this file to the root of a new Cajun Craft Software app repo.
> Replace all `[PLACEHOLDER]` values with app-specific details.
> This template implements the standards defined in
> `cajuncraft/software-development-standards`. Claude's role, startup
> expectations, and lane boundaries are defined canonically in the
> [Development Operating Model](https://github.com/cajuncraft/software-development-standards/blob/main/docs/development-operating-model.md)
> (§3.3, §9.2); this file adds only what is specific to this repository.
>
> **Template-Version:** 1.0

**Instantiates:** `templates/CLAUDE.md` v1.0

---

## 1.0 Repository Purpose

[One paragraph describing what this app does and why it exists.]

This project follows the Cajun Craft Software controlled development model. See
`AI_WORKING_AGREEMENT.md` for the full operating agreement.

---

## 2.0 Safety Rules

Claude Code shall not:

- [APP-SPECIFIC: e.g., delete or overwrite the live/legacy app at `[PATH]`]
- [APP-SPECIFIC: e.g., bind any server to anything other than `127.0.0.1`]
- [APP-SPECIFIC: e.g., expose the app via Cloudflare, NAS, or LAN]
- modify production databases directly
- expose secrets or credentials
- delete backups or repository history
- merge pull requests
- promote to TEST, STAGING, or PROD

---

## 2.1 Ability Is Not Authorization

Claude Code may retain the technical ability to merge, push to `main`, package, expose,
or promote. **That ability is not authorization.** Cajun Craft Software uses capability
with governed authorization: the control is the rule set and Glen's explicit approval,
not the removal of capability.

The Release Gate (§6) and the organization-level list of **protected actions** apply
regardless of what is technically possible in this repo or this session. When a protected
action is technically within reach but not explicitly authorized by Glen for *this*
action, stop and ask. For the canonical protected-actions list and the definition of what
counts as authorization, see the
[Development Operating Model §6 and §6.1](https://github.com/cajuncraft/software-development-standards/blob/main/docs/development-operating-model.md).

---

## 3.0 Allowed Work

Claude Code may:

- improve documentation, tests, and repository structure
- implement features on approved feature branches
- propose migrations or refactors
- write and run tests
- open pull requests

---

## 4.0 Required Workflow

### Session start

1. Read `docs/ai-handoff/current.md`.
2. Extract the active lane name from the **Active Lane:** field.
3. Open `docs/ai-handoff/<lane>-summary.md`.
4. Read the **Next Actions** section.
5. Do not make code changes until Next Actions have been reviewed.

If `docs/ai-handoff/current.md` is absent or the lane summary does not exist, stop and
report to Glen before proceeding.

### Before editing

1. Explain the intended changes.
2. Identify expected files to change.
3. Describe risks and testing approach.

### After editing

1. Summarize changes and tests.
2. Identify known issues and rollback considerations.
3. Update handoff files before stopping:
   - Update **Completed Steps** in the active lane summary.
   - Update **Next Actions** with remaining or newly identified work.
   - Update **Handoff Notes** with context for the next session.
   - Update the **Updated:** date.
   - Update `docs/ai-handoff/current.md` if the active lane changed.

Do not stop without updating the handoff files when code changes were made.

### Engineering judgment within an approved lane

Within an approved scope, Claude Code is expected to exercise independent engineering
judgment — not just execute instructions. This includes:

- Proposing a better technical approach when the approved framing is suboptimal or risky
  (before implementing it, not after).
- Challenging over-prescription: if a lane specifies implementation details that belong
  to engineering judgment, flag it.
- Stopping and escalating if executing the approved scope as written would require
  unsafe, brittle, or technically incorrect work.

Proposing a better approach is not the same as implementing it. Get direction first.

---

## 5.0 Branch Restrictions

Work only in:

```
feature/*
fix/*
docs/*
```

Do not work directly in `main`.

---

## 6.0 Release Gate

Only Glen approves:

- Merging to `main`.
- Promoting to TEST, STAGING, or PROD.
- Any change to what external users can reach.
- Packaging or replacing the live/legacy app.

---

## 7.0 Model Selection

| Model | When to use |
|---|---|
| Sonnet | Default: implementation, bug fixes, documentation, routine review |
| Opus | Architecture, high-risk analysis, security review, complex debugging |
| Haiku | Mechanical cleanup only: typos, formatting, simple renaming |

---

## 8.0 Governance Files and Authority Order

This repo's role files: `ChatGPT.md` (ChatGPT), this file (Claude Code),
`CODEX.md` (Codex), `AGENTS.md` (agent-tooling entry pointer). Role behavior is
defined canonically in
[cajuncraft/software-development-standards](https://github.com/cajuncraft/software-development-standards);
these files carry only repo-local identity, safety rules, and deviations.

On conflicting guidance, the authority order is Operating Model §8.3 —
Glen's explicit instruction, then this repo's safety rules, then the canonical
standards, then role-file guidance, then the active lane prompt. Flag conflicts
to Glen; do not silently resolve them. Claude's own verification evidence
supports a PR but never substitutes for Codex's independent review, and a lane
prompt can narrow scope but never widen authority.
