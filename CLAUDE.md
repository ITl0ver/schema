# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this folder is

`Sqema/` is the **STS Georgian service pages** project — two self-contained static
Georgian web pages for STS, a Tbilisi transport consultancy: a Transport Impact
Assessment (TIA) page and a road-scheme (საგზაო სქემა) page. SEO assets for
`sts.com.ge` subdomains.

## Layout (as built)

Each page is the index of its own subdomain; each folder is an independent deploy root:

```
tia/      → tia.sts.com.ge    index.html · robots.txt · sitemap.xml · fonts/
sqema/    → sqema.sts.com.ge  index.html · robots.txt · sitemap.xml · fonts/
README.md → deploy / DNS / Search Console steps + open items
```

CSS is **inlined** in each page's `<head>` (no shared `styles.css`) — this is the
SEO spec's performance preference (§10) and the clean way to share styling across two
separate subdomain roots. The Georgian font (Noto Sans Georgian) is self-hosted under
each `fonts/` as subsetted woff2 (georgian + latin + latin-ext, one variable file per
subset covering weights 400–700), preloaded with `font-display: swap`.

## Git boundary (important)

The `.git` is rooted at the home directory (`C:\Users\user`), **not** at `Sqema/` —
`Sqema/` is currently untracked, and that repo's origin is `easystat.git`. The STS
build spec (§7) expects this project to be its own repo: when building the STS pages,
run `git init` inside `Sqema/` (or a dedicated subfolder) rather than committing into
the home-directory repo. Do not stage the unrelated EasyStat.ge / home-directory files.

## STS pages — authoritative sources and reading order

The spec is split across files with a strict precedence. Read in this order and treat
later rules as overriding earlier ones **on SEO matters**:

1. `START_HERE_Claude_Code.md` — handoff and entry point.
2. `STS_KA_Pages_Claude_Code_Build_Spec.md` — master build spec: page structure, the
   8-type scheme-checker data model (§5), deploy steps, acceptance criteria (§8).
3. `STS_SEO_Technical_Spec.md` — **authoritative on all SEO; overrides everything else
   where they conflict.** The build must pass its acceptance checklist (§14).
4. `STS_Georgian_LandingPages_and_Ads.md` — verbatim Georgian body copy. Use as-is.

## Non-negotiable architectural constraints (STS pages)

- **SEO Rule #1 (most important):** every piece of content — all 8 scheme types, every
  document line, every price tier, all FAQ text — must exist in **static HTML**. The
  scheme-page checker is **progressive enhancement only**: its JS may show/hide or
  restyle content, but must never be the *sole source* of it. Validate by rendering
  with JS disabled — nothing may disappear.
- **Scheme data lives in the HTML, once.** The 8 scheme types are hand-authored
  `<details class="scheme" id="scheme-N">` blocks in `sqema/index.html` (the source of
  truth, satisfying Rule #1). The checker is a button grid of anchor links to those
  blocks; its tiny inline JS only opens the chosen block and collapses the rest. To
  change a type's documents or pricing, edit its block — the checker needs no changes.
  Pricing models (build spec §5): PERMANENT (types 1,2,4,6), FENCE_TEMP (3), TEMP_TIERED (5,7,8).
- **Vanilla only:** plain HTML + inlined CSS + a small inline vanilla JS. No framework,
  no build step, no browser-storage APIs. Editable by hand.
- **Georgian only, copy verbatim:** all visible text in Georgian; do not rewrite or
  re-translate the supplied copy. Use a Georgian-friendly font (e.g. Noto Sans Georgian).
- **Do not fabricate** prices, phone, email, or legal thresholds. Leave clear `[ ... ]`
  placeholders for unknowns (notably: TIA price line, and scheme types 3 & 8 follow-on
  sub-sections that weren't captured from the source form). Known contacts: phone
  `+995 599 120 755`, email `info@sts.com.ge`, address `3/17 Ana Politkovskaia St,
  Tbilisi 0168`.
- **Anonymous checker:** the checker collects no personal data. File-upload intake lives
  in a downstream Google Form reached via the CTA.
- Per-page `<title>`/meta, single H1 with variant terms as H2/H3, hreflang, FAQPage +
  Organization JSON-LD, `sitemap.xml`, `robots.txt`. Target Lighthouse Performance + SEO ≥95.
