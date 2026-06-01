# START HERE — STS Georgian Pages (Claude Code handoff)

You are building **two self-hosted static Georgian web pages** for STS (a transport consultancy in Tbilisi): a Transport Impact Assessment (TIA) page and a Traffic Organization Scheme page. They are organic-SEO assets, deployed to a static host on a `sts.com.ge` subdomain. SEO is the #1 priority.

## Read these files in this order
1. **STS_KA_Pages_Claude_Code_Build_Spec.md** — the master build spec: pages, structure, the 8-type scheme checker data model, deploy steps, acceptance criteria.
2. **STS_SEO_Technical_Spec.md** — AUTHORITATIVE on all SEO. Overrides everything else on SEO matters. The build must pass its acceptance checklist (note especially Rule #1: all content must be in static HTML, not JS-injected).
3. **STS_Georgian_LandingPages_and_Ads.md** — the verbatim Georgian body copy for both pages (plus an ad/keyword kit you can ignore for the build). Use the copy as-is; do not rewrite or translate.

## How to start
First inspect nothing exists yet — this is a fresh repo. Propose the file structure (one folder: two `.html` files + one `.css`, plus `sitemap.xml`, `robots.txt`, `README.md`) and how you'll build the scheme-page checker so it satisfies SEO Rule #1. Wait for confirmation, then build.

## Open inputs the human still owes you (leave clear `[ ]` placeholders, do not invent)
- TIA service price / turnaround line (scheme prices are already in the build spec §5).
- The two scheme-type follow-on sub-sections not captured (types 3 `სამშენებლო ღობის` and 8 `კომუნიკაციები`).
- Final subdomain name(s) for the CNAME (e.g. `tia.sts.com.ge`, `sqema.sts.com.ge`).
- FAQ Q&A sets per page (5–8 each) if not yet supplied — flag if missing.

## Definition of done
All acceptance criteria in BOTH the build spec (§8) and the SEO Technical Spec (§14) pass. Critical gate: with JavaScript disabled, every scheme type, document list, price, and FAQ is still fully visible in the HTML.
