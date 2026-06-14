# Cajun Craft Software — Development Standards

This repository is the organization-level source of truth for Cajun Craft Software
development standards, AI operating model, product design principles, and default
app shell expectations.

## Contents

### Standards (`docs/`)

| Document | Purpose |
|---|---|
| [Development Operating Model](docs/development-operating-model.md) | Roles, responsibilities, and approval gates for all projects |
| [AI Cross-Check Protocol](docs/ai-cross-check-protocol.md) | How each AI actor monitors drift and calls out process violations |
| [Product Design Principles](docs/product-design-principles.md) | Core UX and product values for all Cajun Craft Software apps |
| [App Shell Standard](docs/app-shell-standard.md) | Required and recommended shell elements for local web apps |
| [Versioning and Environment Standard](docs/versioning-and-environment-standard.md) | How apps expose version, environment, and build identity |
| [Release and Promotion Standard](docs/release-and-promotion-standard.md) | Promotion gates and the role of Glen as PROD gatekeeper |

### Templates (`templates/`)

Starter files to copy into a new Cajun Craft Software app repo:

| Template | Purpose |
|---|---|
| [AI_WORKING_AGREEMENT.md](templates/AI_WORKING_AGREEMENT.md) | Repo-level working agreement for AI-assisted development |
| [CLAUDE.md](templates/CLAUDE.md) | Claude Code instructions tailored for a new CCS app repo |
| [CODEX_REVIEW.md](templates/CODEX_REVIEW.md) | Codex independent-verification checklist |
| [docs/ai-handoff/current.md](templates/docs/ai-handoff/current.md) | Active lane pointer file |
| [docs/ai-handoff/lane-summary-template.md](templates/docs/ai-handoff/lane-summary-template.md) | Lane summary file template |

## Scope

This repository is documentation and templates only. It contains no application source
code, no runtime scripts, and no infrastructure configuration.

## Adoption

Each Cajun Craft Software app repo should:

1. Copy the relevant templates into the new repo root and `docs/ai-handoff/`.
2. Fill in app-specific placeholders (app name, repo path, lane prefix, environment
   variable names).
3. Reference this repo's `docs/` standards for the policies that apply.
4. Keep app-specific customizations in the app repo; keep policy changes here.

## Maintained by

Glen Boudreaux — Cajun Craft Software
