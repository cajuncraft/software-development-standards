# Changelog — Cajun Craft Software Development Standards

Version identity for this repository's standards and role-file templates. Each
canonical doc carries a `**Standard-Version:**` header and each role-file
template a `**Template-Version:**` header; this file records bumps. The
handoff templates (`templates/docs/ai-handoff/`) are unversioned structural
scaffolding by design.

Bump rule (Operating Model §8.4): MAJOR/MINOR bumps to the operating model or a
template trigger one governance-sync docs lane per adopting app repo; PATCH
bumps (typo/clarity) do not.

---

## 2026-07-06 — CCS-004 (implements approved CCS-003)

**New**

- `docs/dev-chat-standard.md` **v1.0** — canonical ChatGPT dev-chat behavior,
  generalized from the Storyteller `ChatGPT.md`.
- `templates/ChatGPT.md` **v1.0** — thin per-repo ChatGPT instance.
- `templates/CODEX.md` **v1.0** — thin per-repo Codex charter + app checks;
  absorbs and replaces `templates/CODEX_REVIEW.md`.
- `templates/AGENTS.md` **v1.0** — agent-tooling entry pointer.
- README: adoption tracker and new-project bootstrap rule.

**Changed**

- `docs/development-operating-model.md` **v1.0** (versioning introduced) —
  added §2.2 lane-crossing rule; sharpened §3.2 (ChatGPT: status with named
  uncertainty, next-actor/model recommendation, one outcome-based prompt; does
  not implement/review/merge/deploy/authorize release) and §3.3 (Claude does
  not self-certify independent review); added §8 governance file set, shared
  vs. local split, authority order, versioning/adoption; §9 per-actor startup
  expectations; §10 Codex Independent-Review Standard.
- `templates/CLAUDE.md` **v1.0** — version header; §8.0 now covers the role-file
  set and authority order.

**Deprecated / removed**

- `templates/CODEX_REVIEW.md` removed (absorbed into `templates/CODEX.md`).
- `templates/AI_WORKING_AGREEMENT.md` deprecated — not for new repos; existing
  instances are slimmed or retired during adoption lanes.

Docs without a version header yet (cross-check protocol, product design
principles, app shell, versioning/environment, release/promotion) remain at
their CCS-002 content; they gain headers when next materially changed.

## Earlier (pre-versioning)

- **CCS-002** — added the "Ability Is Not Authorization" principle across the
  operating model and templates (PR #2).
- **CCS-001** — initial standards and templates (PR #1).
