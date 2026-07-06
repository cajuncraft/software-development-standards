# ChatGPT.md — [APP NAME]

> **Template:** Copy this file to the root of a Cajun Craft Software app repo and
> replace all `[PLACEHOLDER]` values. Keep this file thin — it carries only what
> is specific to this repository. All ChatGPT behavior is defined canonically in
> the [Dev-Chat Standard](https://github.com/cajuncraft/software-development-standards/blob/main/docs/dev-chat-standard.md)
> and the [Development Operating Model](https://github.com/cajuncraft/software-development-standards/blob/main/docs/development-operating-model.md).
>
> **Template-Version:** 1.0

**Instantiates:** `templates/ChatGPT.md` v1.0

---

## 1. Project identity

- **Project:** [APP NAME] ([SHORT NAME])
- **Repository:** `cajuncraft/[REPO-NAME]`
- **Lane prefix:** `[XX]-###`
- **One-line purpose:** [What this app does, in one sentence.]

## 2. Where status lives

- Active lane pointer: `docs/ai-handoff/current.md`
- Lane summaries: `docs/ai-handoff/<lane>-summary.md`
- [APP-SPECIFIC: release/authorization artifacts location, if any]

A `Dev - [APP NAME]` chat that opens cold needs one of these (or working
repository access) before ChatGPT gives recommendations.

## 3. App-specific boundaries ChatGPT must respect when framing work

- [APP-SPECIFIC: e.g., "the legacy app at `[PATH]` is a frozen baseline — no
  lane may propose changing it"]
- [APP-SPECIFIC: e.g., "printing touches real hardware; treat any lane that
  prints as a meaningful boundary"]
- [APP-SPECIFIC: integration boundaries with other Cajun Craft apps]

## 4. Governance file map for this repo

| File | Role |
|---|---|
| `CLAUDE.md` | Claude execution governance — authoritative for gates, safety rules, and release mechanics here |
| `CODEX.md` | Codex review charter and app-specific checks |
| `AGENTS.md` | Agent-tooling entry pointer |

Authority order on any conflict: Operating Model §8.3. Flag conflicts to Glen;
do not silently resolve them.
