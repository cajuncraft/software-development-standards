# AGENTS.md — [APP NAME]

> **Template:** Copy this file to the root of a Cajun Craft Software app repo and
> replace all `[PLACEHOLDER]` values. This file exists for compatibility with
> agent tooling that loads `AGENTS.md` by convention; it stays minimal and
> points to the real charters.
>
> **Template-Version:** 1.0

**Instantiates:** `templates/AGENTS.md` v1.0

---

## Start here

- **Codex:** read [`CODEX.md`](CODEX.md) before any review or scoped task.
- **Claude Code:** governed by [`CLAUDE.md`](CLAUDE.md).
- Canonical operating model:
  [Development Operating Model](https://github.com/cajuncraft/software-development-standards/blob/main/docs/development-operating-model.md).

## Ground rules (all agents)

- Glen is approval authority. GitHub is source of truth. Never expose secrets.
- One agent owns each branch. Codex-owned work uses `codex/*`; Claude uses
  `feature/*`, `fix/*`, or `docs/*`. No agent works directly in `main`.
- Codex does not edit Claude-owned branches without a Glen-approved handoff.
- Without explicit, action-specific Glen approval: no merging, no promotion to
  TEST/STAGING/PROD, no deleting history, no removing rollback capability.
  Ability is not authorization (Operating Model §2.1, §6).
- [APP-SPECIFIC: 1–3 hard prohibitions unique to this repo, e.g., frozen
  baselines, hardware actions, live data paths]
