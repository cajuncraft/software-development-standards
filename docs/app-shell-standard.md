# Cajun Craft Software UI Shell Standard

**Scope:** All Cajun Craft Software local web applications
**Maintained in:** `cajuncraft/software-development-standards`
**Standard-Version:** 2.0 *(CCS-006 major revision; versioning introduced at 2.0 — see [CHANGELOG](../CHANGELOG.md))*
**Companion standard:** CCS-005 — App identity and icon family (issue #7; icon-family rules live there once published)

---

## 1. Purpose and status

This document defines the shared application shell for Cajun Craft Software apps:
identity surfaces, sidebar/header layout, visual theme, About page, device icons,
mobile behavior, legacy-name handling, required regression tests, and review gates.
The goal is that every Cajun Craft app is immediately recognizable as family —
same shell skeleton, same identity placement, same badges — while each product
stays visually distinct through its own name, short code, icon glyph, and
navigation sections.

**Reference direction:** CC-INV (Cajun Craft Inventory) as of the HI-068 (rename +
About + favicon), HI-070 (sidebar icon mark), and HI-071 (Apple touch icon) lanes is
the current reference shell. It is a worked example under active refinement, not a
frozen pixel spec: details may be tuned before broad adoption, and this standard is
the contract, not any single file in a product repo.

**Supersedes:** version 1 of this document (the VG-era shell standard). The GPB Voice
Generator remains a valid v1-era implementation until its own adoption lane; §11 covers
adoption sequencing. Where v1 and this version conflict (notably environment badge
colors, §6.4), this version wins.

Relationship to **CCS-005**: CCS-005 defines *what the product identity and icon family
look like* (tile geometry, palette, glyph rules, per-product marks). This standard
defines *where and how the shell uses them* (sidebar mark, favicon, Apple touch icon,
About page). Shell lanes must consult both.

---

## 2. App identity surfaces

### 2.1 Naming pattern

- **Full product name:** `Cajun Craft <Product>` — e.g. `Cajun Craft Inventory`,
  `Cajun Craft Print Library`.
- **Short code:** `CC-<XX>` — e.g. `CC-INV`, `CC-PL`, `CC-VG`, `CC-ST`. Short codes are
  what fits where space is tight (sidebar brand, home-screen shortcut name).
- **Legacy/internal codename:** when a product has been renamed (e.g. CC-INV was
  CC-SILS), the old name survives only as a documented internal codename (§8).

### 2.2 Identity constants — single source of truth

Each app defines its identity once, in code, and injects it into every template
(context processor or equivalent):

```text
APP_NAME             e.g. "Cajun Craft Inventory"
APP_SHORT_NAME       e.g. "CC-INV"
APP_LEGACY_CODENAME  e.g. "CC-SILS" (omit/None when not applicable)
APP_VERSION          single source of truth, vMAJOR.MINOR.PATCH
APP_ENV              DEV | TEST | PROD (from env var; unknown defaults to DEV)
```

Templates must never hardcode the name, short code, or version outside these
constants. (The HI-070 lesson: a hardcoded `CC` sidebar tile silently survived a
rename.)

### 2.3 Sidebar app mark (top-left)

- The mark is the **product icon** (per CCS-005), rendered as an image — never a
  hardcoded text tile.
- It must be sourced from the **same asset as the browser favicon** (or the same
  source-of-truth glyph) so the browser icon and the visible app mark cannot drift.
- Beside the mark: the short code in bold and the version underneath, with the full
  product name available on the brand link (e.g. `title` attribute / tooltip).
- The brand block links to the app's home page.

### 2.4 Browser favicon

- A source-controlled SVG favicon (`static/favicon.svg` or equivalent), linked from the
  shell with cache-busting tied to `APP_VERSION`.
- Design per CCS-005 (rounded dark tile, gold Cajun Craft accent, simple product glyph,
  readable at 16 px).

### 2.5 Apple / iOS home-screen icon

- A source-controlled **180×180 PNG** `apple-touch-icon` derived from the same icon
  source, linked with `sizes="180x180"` and cache-busting.
- The PNG must be **full-bleed** (no transparent rounded corners): iOS composites
  transparency over black and applies its own corner mask.
- Include `<meta name="apple-mobile-web-app-title" content="{{ APP_SHORT_NAME }}">` so
  the suggested shortcut name is the short code.
- Icon binaries are committed only through an approved product lane; document the
  regeneration method (source SVG → 180×180 render) in the lane handoff.

### 2.6 Web app manifest

Optional. A minimal manifest is acceptable when it clearly helps (e.g. Android
home-screen installs); a full PWA (service worker, offline) is **not** a shell
requirement and needs its own lane.

### 2.7 Version and environment display

- **Version:** in the sidebar brand block (§2.3), on the About page, and in the
  status/health surface where one exists.
- **Environment badge:** visible on **every page** in the top bar — solid,
  high-contrast, showing `DEV`/`TEST`/`PROD`, with a tooltip of the form
  `Environment: <ENV> · Version <VERSION>`.
- When a release does not bump `APP_VERSION` (e.g. a visual-only fix), the deployed
  SHA is the build discriminator; promotion records must say so.

---

## 3. Layout

```text
+------------+-----------------------------------------------+
| sidebar    | top bar (~56px):                              |
| 240px      |  [mobile menu] [page title] ...spacer...      |
| (56px icon |  [env badge] [theme toggle]                   |
|  rail when +-----------------------------------------------+
|  unpinned) |                                               |
|            |  main content (themed, flex-fill, cards)      |
| [pin]      |                                               |
| ...nav...  |                                               |
| footer:    |                                               |
| Cajun Craft|                                               |
| Software   |                                               |
+------------+-----------------------------------------------+
```

- The **sidebar always uses dark chrome**, regardless of the content theme.
- The content area follows the active theme tokens (§6).
- Sidebar footer carries the vendor byline **Cajun Craft Software**.

---

## 4. Navigation

- **Grouped nav** with small uppercase section titles (e.g. FIND / INVENTORY /
  SYSTEM), each item an icon + label.
- Icons come from a local inline SVG sprite (line icons, `currentColor`) — no icon
  fonts, no CDN (§7 of v1 carried forward: local-first assets, external assets need
  explicit approval).
- **Active item** is visually distinct (accent background/left edge) and carries
  `aria-current="page"`.
- **About is the last navigation item.** Always. This is a family-recognition anchor
  and a regression-test target.
- Desktop: sidebar is pinnable; unpinned it collapses to an icon rail with
  hover/focus expansion. Pin state persists in `localStorage`.
- **Mobile (phone-width): the sidebar is a drawer, collapsed by default**, opened by
  an obvious top-bar menu button, with a backdrop, tap-outside-to-close,
  link-tap-to-close, and Escape-to-close.

---

## 5. About page

Every app has an About page (last nav item) presenting, at minimum:

| Field | Content |
|---|---|
| Application | `APP_NAME (APP_SHORT_NAME)` with short-code chip |
| Version | `APP_VERSION` |
| Environment | env badge |
| Legacy / internal codename | when applicable — see §8 wording pattern |
| Publisher | Cajun Craft Software |
| Copyright | `© <YEAR> Cajun Craft Software. All rights reserved.` |
| Purpose | one short paragraph of what the app manages |
| Help / Getting Started | section or link |
| Runtime notes | local-only/security-boundary statement where applicable |

Operational/support notes are welcome; secrets, keys, and credential material are
never rendered.

---

## 6. Visual theme

### 6.1 Baseline

Shared **dark shell chrome** (sidebar/topbar) over a themeable content area. Apps that
support themes offer **System / Light / Dark**, persist the choice in `localStorage`,
and run a pre-paint script so there is no theme flash on load.

### 6.2 Color tokens

Content styling uses CSS custom properties with these canonical names (the CC-INV
`style.css` token set is the reference):

```text
--color-surface / --color-surface-3        panels, inset surfaces
--color-text / --color-text-muted / --color-text-faint
--color-border / --color-border-strong
--color-primary / --color-primary-text     buttons
--color-accent / --color-accent-soft       focus, active nav, highlights
--color-ok / --color-warn / --color-danger (+ matching *-bg variants)
```

Apps may extend the palette; they should not rename these tokens.

### 6.3 Cajun Craft gold

The brand gold (reference value `#c9a227`, canonical definition in CCS-005) is
reserved for **brand and icon accents** — the icon tile ring/glyph accents and
sparing brand moments. It is not a general-purpose UI accent color.

### 6.4 Environment badge colors

Solid, white-text badges (supersedes the v1 table):

| Environment | Reference color |
|---|---|
| PROD | green (`#1b7a3d`; dark-theme `#2e9b58`) |
| TEST | amber (`#a8762a`; dark-theme `#c08a36`) |
| DEV | slate (`#4b566b`; dark-theme `#56627a`) |

Rationale: PROD reads as "the healthy real system", not as an alarm; TEST warns;
DEV recedes.

### 6.5 Components

- **Cards:** rounded panels (`--color-surface`, 1px border) as the primary content
  container; page = stacked cards.
- **Buttons:** primary (filled `--color-primary`), secondary (surface + border), and
  warn variants; shared radius token; whole-button click targets.
- **Badges/pills:** small bold pills — status family `badge--ok / --warn / --issue /
  --neutral / --pending / --complete` plus the solid env badges (§6.4).
- **Radius/spacing/typography:** one pill radius and one button/card radius token;
  `.row` (flex, wraps) and `.stack` (grid gap) utility layouts; system font stack;
  compact scale (~0.72rem badges → ~0.95rem body → modest headings).
- Tables collapse to labeled rows on mobile (`data-label` pattern) or an equivalent
  responsive strategy.

---

## 7. Local-first assets

Carried forward from v1 unchanged in substance: shell CSS/JS/fonts/images are served
by the app itself; CDN assets, web fonts, and analytics require Glen's explicit
approval; fully-local apps should have a check that no unexpected external URLs exist
in web assets. Approved integrations are fine — the rule prevents *accidental*
external dependencies.

---

## 8. Legacy naming and operational safety

When a product's user-facing name changes:

- **User-facing surfaces** (shell, titles, About, prose, exports, docs meant for
  users) are renamed consistently in a dedicated lane.
- **Operational identifiers are never renamed by a shell/rename lane:** repository
  names, Docker compose/project/container/image names, database files and tables,
  environment variables, credential/app IDs, ports, URLs, backup paths, service
  names, API paths, and log formats. Renaming any of these requires its own
  explicitly-approved lane.
- The old name may deliberately survive user-facing in narrow, documented contexts
  (e.g. CC-INV's "CC-SILS ID" label nomenclature kept for printed-label continuity).
- Every rename PR must include a **classification of remaining old-name
  occurrences**: intentionally operational · deliberate user-facing legacy ·
  docs/history · missed rename (must be zero).
- About page wording pattern: *"Legacy/internal codename: `<OLD>` — formerly the
  app's display name. It remains in operational identifiers (…) for stability and
  support history."*

---

## 9. Starter / template expectations

Future apps adopt the shell as a **pattern, not a file clone** — Flask, FastAPI, or
another stack can all comply. The starter surface every new Cajun Craft app begins
from (bootstrap requirement, same footing as the role-file set):

```text
Cajun Craft Software App Shell
├── base layout / sidebar / topbar pattern      (§3–4)
├── product identity constants                  (§2.2, injected globally)
├── shared CSS/theme tokens                     (§6)
├── About page pattern                          (§5, last nav item)
├── favicon + apple-touch-icon pattern          (§2.4–2.5)
├── environment/version badges                  (§2.7)
├── mobile sidebar behavior                     (§4)
└── shell identity regression tests             (§10)
```

A product supplies only:

```text
APP_NAME · APP_SHORT_NAME · APP_LEGACY_CODENAME (if any)
APP_ICON_SOURCE / glyph direction (per CCS-005)
navigation sections · product-specific pages
```

Until a packaged starter exists, **CC-INV is the copy-from reference** (its
`base.html`, `style.css` token set, About page, icon assets, and
`tests/test_app_identity.py`). If a reusable starter package/template repo is later
extracted, it lives under this standards repo's `templates/` or a dedicated starter
repo via its own CCS lane.

---

## 10. Required shell regression tests

Every product repo includes tests (or equivalent enforced checks) that:

1. the sidebar mark is the product icon image and **not a stale generic text tile**;
2. browser favicon metadata exists and points to a source-controlled asset;
3. Apple touch icon metadata exists, points to a source-controlled asset, and the
   asset is a real 180×180 PNG;
4. app name, short code, version, and environment badge render in the shell;
5. the About page renders the §5 identity fields (including the legacy codename note
   when applicable);
6. **About is the last nav item**;
7. mobile sidebar defaults to collapsed where applicable (markup/state-level check);
8. stale legacy names do not appear in normal user-facing UI outside the explicitly
   allowed contexts (a sweep across representative pages with an allowlist);
9. shell/GET rendering causes no data side effects (identity pages are read-only).

CC-INV's `tests/test_app_identity.py` is the reference implementation of this suite.

---

## 11. Review gates and adoption

### 11.1 Codex / Claude review checklist

Every new-app bootstrap, shell-change, or rename PR must be reviewed against:

> **Does this comply with the Cajun Craft Software UI shell standard (CCS-006)?**

Specifically:

- [ ] identity constants are the single source of truth (no hardcoded name/code/version)
- [ ] sidebar mark = product icon image, shared source with favicon
- [ ] favicon + apple-touch-icon links present; assets source-controlled
- [ ] version + env badge placement per §2.7
- [ ] About present, complete (§5), and last in nav
- [ ] mobile drawer behavior intact
- [ ] theme tokens used, not renamed; gold reserved per §6.3
- [ ] legacy-name classification included (renames) — no missed user-facing renames
- [ ] no operational identifier renamed
- [ ] §10 regression tests present/updated and passing

Product-repo `CODEX.md` app checks should incorporate this question when the repo
adopts the standard.

### 11.2 Adoption sequencing

- **CC-INV** — current reference direction (HI-068/070/071); refinements to the
  reference happen in normal CC-INV lanes and feed back into this standard.
- **CC-PL** — first planned adoption target, via **PL-012** (shell alignment lane in
  `cajuncraft-print-library`), citing this standard directly.
- **VG / ST** — later adoption lanes; v1-era shells remain acceptable until their
  lanes run.
- Adoption is tracked in the README adoption tracker alongside the role-file rows.

Per Operating Model §8.4, this MAJOR revision triggers governance-sync/adoption
lanes per app repo — scheduled by Glen, not automatically.
