# STS — Georgian Service Pages (TIA + Road Scheme)

Two self-contained static Georgian pages, organic-SEO assets for `sts.com.ge`
subdomains. No framework, no build step — plain HTML with inlined CSS, vanilla JS,
and self-hosted fonts. Edit by hand and `git push`.

```
tia/      → deploy root for  tia.sts.com.ge   (Transport Impact Assessment / TIA)
sqema/    → deploy root for  sqema.sts.com.ge (road-scheme page + interactive checker)
```

Each folder is an independent, fully deployable site: `index.html`, `robots.txt`,
`sitemap.xml`, and a `fonts/` directory with the subsetted Noto Sans Georgian woff2
files (georgian + latin + latin-ext, weights 400–700 via one variable file each).

## Key design facts

- **All content is static HTML** (SEO Rule #1). On the scheme page the full 8-type
  guide — every type, document, and price tier — lives in a `<details>` accordion in
  the markup. The checker JS is **progressive enhancement only**: it opens the chosen
  type and collapses the rest. With JS disabled, the checker buttons are plain anchor
  links and every word is still visible. Verify with JS off before shipping.
- **CSS is inlined** in each page's `<head>` (zero render-blocking CSS); JS is a tiny
  inline script at end of body. Fonts are self-hosted and preloaded with
  `font-display: swap`.
- **FAQ accordions use native `<details>`** so the Q&A text is in the HTML and matches
  the FAQPage JSON-LD exactly.

## Deploy (Netlify / Cloudflare Pages / GitHub Pages)

Because the two pages are separate subdomains, deploy each folder as its own site/root.

1. Push this repo to GitHub (already wired to `github.com/Urbanoise/sqema`).
2. Create **two** sites on the host (or one project per subdomain), with publish
   directories `tia/` and `sqema/` respectively.
3. **DNS (one-time, whoever controls sts.com.ge DNS):** add CNAME records
   - `tia`   → host target for the TIA site
   - `sqema` → host target for the scheme site
4. Host compression (Brotli/gzip) and long cache headers on static assets are on by
   default on Netlify/Cloudflare — leave enabled.

## After deploy

- **Google Search Console:** verify each subdomain (DNS TXT), submit each
  `sitemap.xml`, watch Coverage + Performance + Core Web Vitals.
- **Validate:** Rich Results Test (all JSON-LD blocks), Lighthouse (Performance + SEO
  ≥ 95 on mobile), Mobile-Friendly check, and a **JS-off render check** (Rule #1).
- **Google Business Profile** in Georgian — NAP identical to the site.
- **Authority bridge (important):** add links FROM `sts.com.ge` (e.g. the Services
  page) TO `tia.sts.com.ge` and `sqema.sts.com.ge` with descriptive Georgian anchor
  text. A subdomain doesn't auto-inherit the main domain's trust — these internal
  links are how authority crosses. (One-time action for ITLover / Nikoloz.)

## Open items to fill before/after launch

These are intentionally left as visible `[ ... ]` placeholders in the HTML — do not
fabricate; replace with real values.

- **TIA turnaround/price:** `[X სამუშაო დღე]` on the TIA page (hero "რატომ STS" line and
  the first two FAQ answers). The 1-day quote turnaround is already stated; only the
  delivery turnaround/price is open.
- **Scheme page turnaround:** `[1 დღეში]` / `[X სამუშაო დღეში]` in "როგორ ვმუშაობთ" and FAQ.
- **Scheme types 3 & 8 follow-on sub-sections:** the source form has an extra gated
  sub-section after "სამშენებლო ღობე" (type 3) and "კომუნიკაციები" (type 8) that wasn't
  captured. Each is marked with a `[დასაზუსტებელია]` warning box in the accordion —
  Nikoloz to supply the fields.
- **More FAQs (SEO depth):** the SEO spec recommends 5–8 FAQ items per page; the
  supplied verbatim copy has 4 (TIA) and 3 (scheme). Additional client-approved Q&As
  targeting long-tail queries (`რა ღირს`, `როდის სავალდებულო`, `ვინ ამტკიცებს`) would help.
- **OG/hero image:** `og:image` currently points to the real STS logo on sts.com.ge.
  A purpose-made share image (WebP, descriptive Latin filename, Georgian alt) is optional.
- **hreflang `en`:** omitted by design — add a reciprocal `en` alternate only if a true
  English equivalent page exists on sts.com.ge (coordinate with ITLover).

## Editing the scheme data

The 8 scheme types are hand-authored HTML in `sqema/index.html` (the source of truth,
per SEO Rule #1). To change a type's documents or pricing, edit its `<details
class="scheme" id="scheme-N">` block directly. The checker needs no data changes — it
just targets these blocks by `id`. Pricing models in the build spec: PERMANENT (types
1,2,4,6), FENCE_TEMP (type 3), TEMP_TIERED (types 5,7,8).
