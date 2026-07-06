# CODEX.md — [APP NAME]

> **Template:** Copy this file to the root of a Cajun Craft Software app repo and
> replace all `[PLACEHOLDER]` values. Keep this file thin — Codex's role,
> startup expectations, and the validity bar for a review are defined
> canonically in the
> [Development Operating Model](https://github.com/cajuncraft/software-development-standards/blob/main/docs/development-operating-model.md)
> (§3.4, §9.3, and **§10 Codex Independent-Review Standard**). This file adds
> only what is specific to this repository.
>
> This file absorbs and replaces the earlier `CODEX_REVIEW.md` template.
>
> **Template-Version:** 1.0

**Instantiates:** `templates/CODEX.md` v1.0

---

## 1. Charter (summary — canonical text governs)

Codex independently verifies Claude's work: it inspects the actual diff and
surrounding code, runs the tests itself, verifies the PR's meaningful claims,
distinguishes **blocking** findings from **non-blocking observations**, and
recommends merge / merge-with-notes / do-not-merge. A completed review is a
recommendation only — **merge and promotion require Glen's explicit, per-action
approval.** Repeating the PR summary back with boxes ticked is not a review
(Operating Model §10).

## 2. How to review in this repo

- Check out the PR branch in an isolated worktree; confirm it targets `main`.
- Test command: `[APP-SPECIFIC: exact command, e.g. .venv\Scripts\python.exe -m pytest tests/ -q]`
  — never rely on the PR's reported test output.
- Handoff files to verify: `docs/ai-handoff/current.md` and the lane summary
  were updated per the repo's `CLAUDE.md`.
- [APP-SPECIFIC: environment/venv notes, paths that must never be used]

## 3. App-specific review checks

In addition to the §10 standard, verify for every PR:

- [ ] [APP-SPECIFIC: e.g., frozen baseline `[PATH]` untouched]
- [ ] [APP-SPECIFIC: e.g., no binding beyond `127.0.0.1`; no external assets in `web/`]
- [ ] [APP-SPECIFIC: e.g., no real printing triggered; no production data paths touched]
- [ ] No hardcoded secrets, credentials, or tokens; no new external dependencies
      without Glen's approval.
- [ ] No protected action (merge, push to `main`, promote, package, expose,
      runtime change, legacy replacement — Operating Model §6) taken or staged
      on the basis of technical access alone.

## 4. Ops lane (if this repo has one)

[APP-SPECIFIC: pointer to the ops-lane doc, e.g. `docs/codex-ops.md`, or
"This repo has no Codex ops lane." Scoped operational tasks run only under an
explicit Glen-approved task with named stop points.]

## 5. Review report format

Every review ends with:

```text
Date / branch / commit SHA reviewed
Test command run + observed result
Claims verified (list) / claims not verifiable (list)
Blocking findings (or "none")
Non-blocking observations (or "none")
Recommendation: merge | merge with notes | do NOT merge
```

**Merge only after Glen's explicit approval** — technical ability to merge is
not authorization.
