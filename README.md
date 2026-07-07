# Cajun Craft Software — Development Standards

This repository is the organization-level source of truth for Cajun Craft Software
development standards, AI operating model, product design principles, and default
app shell expectations.

The canonical standards and the role-file templates are versioned; see
[CHANGELOG.md](CHANGELOG.md) and the `Standard-Version` / `Template-Version`
headers in those files. The handoff templates are unversioned structural
scaffolding — their instances are per-lane working files, not governance
documents. Policy changes happen only in this repository — never as edits to
an app repo's local copy (Operating Model §8.4).

## Contents

### Standards (`docs/`)

| Document | Purpose |
|---|---|
| [Development Operating Model](docs/development-operating-model.md) | Roles, approval gates, authority order, startup expectations, and the Codex review standard for all projects |
| [Dev-Chat Standard](docs/dev-chat-standard.md) | How ChatGPT behaves in `Dev -` conversations: status with named uncertainty, next-actor/model recommendation, outcome-based prompts |
| [AI Cross-Check Protocol](docs/ai-cross-check-protocol.md) | How each AI actor monitors drift and calls out process violations |
| [Product Design Principles](docs/product-design-principles.md) | Core UX and product values for all Cajun Craft Software apps |
| [App Shell Standard](docs/app-shell-standard.md) | Required and recommended shell elements for local web apps |
| [Versioning and Environment Standard](docs/versioning-and-environment-standard.md) | How apps expose version, environment, and build identity |
| [Release and Promotion Standard](docs/release-and-promotion-standard.md) | Promotion gates and the role of Glen as PROD gatekeeper |

### Templates (`templates/`)

Role files and handoff structure to copy into a Cajun Craft Software app repo.
Templates are deliberately **thin**: role behavior lives in the standards above;
the local copies carry only app identity, safety rules, and deviations.

| Template | Reader | Purpose |
|---|---|---|
| [ChatGPT.md](templates/ChatGPT.md) | ChatGPT | App identity, status locations, app-specific product boundaries |
| [CLAUDE.md](templates/CLAUDE.md) | Claude Code | App safety rules, workflow, branch rules, release gate |
| [CODEX.md](templates/CODEX.md) | Codex | App-specific review checks, test command, report format |
| [AGENTS.md](templates/AGENTS.md) | Agent tooling | Compatibility entry pointer + ground rules |
| [docs/ai-handoff/current.md](templates/docs/ai-handoff/current.md) | all | Active lane pointer file *(unversioned scaffolding)* |
| [docs/ai-handoff/lane-summary-template.md](templates/docs/ai-handoff/lane-summary-template.md) | all | Lane summary file template *(unversioned scaffolding)* |

Deprecated: [AI_WORKING_AGREEMENT.md](templates/AI_WORKING_AGREEMENT.md) — superseded
by the role-file set; not for new repos. `CODEX_REVIEW.md` was absorbed into
`CODEX.md` (CCS-004).

## Scope

This repository is documentation and templates only. It contains no application source
code, no runtime scripts, and no infrastructure configuration.

## New-Project Bootstrap Rule

A new Cajun Craft Software repository is **not ready for its first
implementation lane** until it contains, instantiated from the current
templates with placeholders filled:

1. `ChatGPT.md`, `CLAUDE.md`, `CODEX.md`, and `AGENTS.md` at the repo root;
2. `docs/ai-handoff/current.md` and the lane-summary template;
3. a row in the adoption table below recording the instantiated template
   versions.

The project's scaffold lane must include these. Per the Dev-Chat Standard,
ChatGPT should not recommend implementation lanes for a repo missing them. No
monorepo or sync tooling is involved — version headers and the table below are
the whole mechanism.

## Adoption Tracker

| App repo | ChatGPT.md | CLAUDE.md | CODEX.md | AGENTS.md | Governance-sync lane |
|---|---|---|---|---|---|
| `home-inventory-system` (CC-SILS) | pending | pre-1.0 (app-specific) | pending | pre-1.0 (app-specific) | pending |
| `gpb-voice-generator` | pending | pre-1.0 (app-specific) | pending (has legacy `CODEX_REVIEW.md`) | pending | pending |
| `gpb-storyteller` | pre-1.0 (org-generic — to be thinned) | pre-1.0 (app-specific) | pending | pre-1.0 (app-specific) | pending |
| `cajuncraft-print-library` (CC-PL, future) | — | — | — | — | bootstraps complete at PL-001 |

Adoption PRs update their own row. "pre-1.0" marks a file that predates this
versioned template set and is aligned during that repo's governance-sync lane.

## Adoption (per repo)

1. Copy the role-file templates and `docs/ai-handoff/` templates into the repo.
2. Fill in app-specific placeholders (app name, repo, lane prefix, safety rules,
   test command).
3. Keep app-specific customizations in the app repo; make policy changes here.
4. Update the adoption tracker row in this README in the same PR.

## Maintained by

Glen Boudreaux — Cajun Craft Software
