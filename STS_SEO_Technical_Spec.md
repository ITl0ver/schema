# SEO Technical Specification — STS Georgian Pages
### Authoritative SEO addendum to `STS_KA_Pages_Claude_Code_Build_Spec.md`. Where this file and any other conflict, **this file wins on all SEO matters.**

Goal: rank #1 on google.ge for low-competition, high-intent Georgian terms. Google Ads can't run in Georgian, so organic Search is the entire acquisition channel — every page must be technically flawless. The competition is thin and unoptimized; clean execution wins.

---

## RULE #1 — JS-rendered content does NOT get indexed reliably. (Most important rule in this file.)
This is a **static site with no server-side rendering.** Googlebot indexes what is physically in the `.html` file on first load.

- **Every word of indexable content must be real HTML in the file** — present in "View Source", not injected by JavaScript.
- The scheme-page **checker is progressive enhancement only.** The full 8-type guide (every type name, every document line, every price tier, all the FAQ text) must exist as a **static HTML accordion** in the page. The checker JS may show/hide or restyle that content, but must never be the *sole source* of it.
- Test: disable JavaScript → the entire guide, all 8 types, all FAQs, all body copy must still be fully visible and readable. If anything disappears, it won't rank. Fix before shipping.
- Do not lazy-render text content. Lazy-loading applies to below-the-fold *images* only, never text.

---

## 1. Keyword → page mapping (prevent cannibalization)
Each target term lives on exactly ONE page. Never target the same head term on both.

**TIA page** — primary `ტრანსპორტის ზემოქმედების შეფასება`; secondary/variants (use as H2/H3 + in body, naturally): `ტრანსპორტზე ზემოქმედების შეფასება`, `ტრანსპორტის ზეგავლენის შეფასება`, `ტრანსპორტზე ზეგავლენის შეფასება`, `სატრანსპორტო კვლევა`. Intent: regulatory/transactional ("I need this for my permit").

**Scheme page** — primary `საგზაო მოძრაობის ორგანიზების სქემა`; variants: `დროებითი სქემა`, `მუდმივი სქემა`, `საგზაო მოძრაობის ორგანიზების დროებითი სქემა`, `საგზაო მოძრაობის ორგანიზების მუდმივი სქემა`, `ტრანსპორტის ორგანიზების სქემა`, `გზის გადაკეტვის სქემა`, `საგზაო სქემის დამზადება`. The 8 scheme-type names are themselves long-tail keywords — keep each as visible HTML text.

---

## 2. URLs / slugs
- Latin transliteration, lowercase, hyphenated. **Never Georgian script in the URL** (it percent-encodes into garbage when shared/linked).
  - TIA: `/transportis-zemoqmedebis-shefaseba` (or root of `tia.` subdomain)
  - Scheme: `/sagzao-skema` (or root of `sqema.` subdomain)
- Pick trailing-slash convention and stick to it; 301 the other variant if the host allows.
- One canonical URL per page (see §7).

---

## 3. `<head>` — exact tags (per page)
Order in `<head>`: charset → viewport → title → description → canonical → robots → hreflang → Open Graph → Twitter → preconnect/preload → JSON-LD.

```html
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>{TITLE}</title>
<meta name="description" content="{DESC}">
<link rel="canonical" href="{SELF_URL}">
<meta name="robots" content="index,follow,max-image-preview:large,max-snippet:-1">
<link rel="alternate" hreflang="ka" href="{SELF_URL}">
<link rel="alternate" hreflang="en" href="{EN_EQUIVALENT_OR_OMIT}">
<link rel="alternate" hreflang="x-default" href="{SELF_URL}">
<meta property="og:type" content="website">
<meta property="og:locale" content="ka_GE">
<meta property="og:title" content="{TITLE}">
<meta property="og:description" content="{DESC}">
<meta property="og:url" content="{SELF_URL}">
<meta property="og:image" content="{ABSOLUTE_IMAGE_URL}">
<meta name="twitter:card" content="summary_large_image">
```

**TITLE / DESC values** (Georgian glyphs are wide — these are sized to not truncate):
- TIA TITLE: `ტრანსპორტის ზემოქმედების შეფასება — სწრაფად | STS`
- TIA DESC: `ტრანსპორტის ზემოქმედების შეფასება (TIA) სამშენებლო ნებართვისთვის — სწრაფად და ხელმისაწვდომ ფასად. მიიღეთ შეთავაზება 1 დღეში.`
- Scheme TITLE: `საგზაო მოძრაობის ორგანიზების სქემა — დროებითი/მუდმივი | STS`
- Scheme DESC: `საგზაო მოძრაობის ორგანიზების სქემა (დროებითი და მუდმივი) მშენებლობისა და სამუშაოებისთვის. დასამტკიცებლად მზა, სწრაფად.`

hreflang note: only emit the `en` alternate if a true English equivalent page exists (e.g. the TIA page maps to sts.com.ge's English Traffic Impact Assessment page). If none, drop the `en` line and keep `ka` + `x-default` self-referential. hreflang must be reciprocal — the English page should point back, so coordinate with ITLover or omit.

---

## 4. Heading hierarchy
- Exactly **one `<h1>`** per page = the primary keyword phrase.
- Logical nesting: `h1 → h2 → h3`, never skip levels, never use headings for styling.
- Put the primary keyword in the H1 and within the **first 100 words** of body text.
- Use the variant terms (§1) as H2/H3 of real sections — not stuffed, each heading introduces genuine content.
- Use semantic HTML5: `<header> <nav> <main> <section> <article> <footer>`, one `<main>` per page.

---

## 5. Structured data (JSON-LD in `<head>` or end of `<body>`)
Validate every block in Google Rich Results Test before shipping. Use this real STS data.

**A. ProfessionalService / Organization — on BOTH pages:**
```json
{
  "@context": "https://schema.org",
  "@type": "ProfessionalService",
  "@id": "https://sts.com.ge/#organization",
  "name": "STS — Smart Transportation Solutions",
  "url": "https://sts.com.ge/",
  "logo": "https://sts.com.ge/wp-content/uploads/2025/05/STS-Logo-Dark-Version-v2.png",
  "image": "https://sts.com.ge/wp-content/uploads/2025/05/STS-Logo-Dark-Version-v2.png",
  "telephone": "+995577155575",
  "email": "info@sts.com.ge",
  "address": {
    "@type": "PostalAddress",
    "streetAddress": "3/17 Ana Politkovskaia St",
    "addressLocality": "Tbilisi",
    "postalCode": "0168",
    "addressCountry": "GE"
  },
  "geo": { "@type": "GeoCoordinates", "latitude": 41.7213178, "longitude": 44.7142103 },
  "areaServed": { "@type": "Country", "name": "Georgia" },
  "sameAs": ["https://www.linkedin.com/company/stscomge/"]
}
```

**B. Service — one per page (Georgian serviceType):**
```json
{
  "@context": "https://schema.org",
  "@type": "Service",
  "serviceType": "ტრანსპორტის ზემოქმედების შეფასება",
  "provider": { "@id": "https://sts.com.ge/#organization" },
  "areaServed": { "@type": "Country", "name": "Georgia" },
  "description": "{one-sentence Georgian description of the service}"
}
```
(Scheme page: `serviceType: "საგზაო მოძრაობის ორგანიზების სქემა"`.)

**C. FAQPage — one per page, built from that page's FAQ Q&As:**
```json
{
  "@context": "https://schema.org",
  "@type": "FAQPage",
  "mainEntity": [
    { "@type": "Question", "name": "{Georgian question}",
      "acceptedAnswer": { "@type": "Answer", "text": "{Georgian answer}" } }
  ]
}
```
The JSON-LD FAQ text MUST match the visible on-page FAQ text exactly (Google penalizes mismatched/ hidden FAQ markup).

**D. BreadcrumbList — optional but recommended** (`მთავარი › {service}`).

---

## 6. On-page content & E-E-A-T
- **Depth:** aim ≥600–900 Georgian words of genuine body copy per page (the scheme page easily exceeds this via the 8-type guide). Thin pages don't rank; the static guide is your depth.
- **FAQ = long-tail capture.** Each Q targets a real query (`რა ღირს...`, `როდის არის სავალდებულო`, `რამდენ ხანში მზადდება`, `ვინ ამტკიცებს`). Add 5–8 per page.
- **Semantic richness:** naturally include related terms (ნებართვა, მერია, საპატრულო პოლიცია, DWG, ტოპოგრაფიული გეგმა, საკადასტრო კოდი, მშენებლობა, ტროტუარი) — they reinforce topical relevance. Never keyword-stuff; write for humans.
- **Trust signals (E-E-A-T):** show NAP (name/address/phone) in the footer as real text, link the LinkedIn, reference ADB/GIZ-level experience and the professional toolset, name the company. Real contact info + address is a strong local-trust signal.
- **Freshness:** include a visible/last-updated cue if practical; keep the FAQ current.

---

## 7. Canonical, robots.txt, sitemap
- Self-referencing `<link rel="canonical">` on every page (absolute URL).
- `robots.txt`:
```
User-agent: *
Allow: /
Sitemap: {SUBDOMAIN}/sitemap.xml
```
- `sitemap.xml` listing both pages with `<lastmod>`; submit in Google Search Console.
- Ensure NOTHING blocks crawl (no stray `noindex`, no `Disallow`). Static hosts sometimes ship a default — verify.

---

## 8. Internal & external linking (authority flow)
- **Critical (subdomain authority bridge):** add links FROM the main site `sts.com.ge` (e.g. the Services page) TO these Georgian pages, with descriptive Georgian anchor text. A subdomain doesn't auto-inherit the main domain's trust — these internal links are how authority crosses. One-time action for ITLover/Nikoloz.
- Georgian TIA page ⇄ scheme page (contextual links, keyword-rich anchors, not "click here").
- Link both pages back to `https://sts.com.ge/`.
- Later: support articles (`როდის არის სავალდებულო?` etc.) each linking down to the money page with descriptive anchors.

---

## 9. Image SEO
- Format **WebP** (or AVIF); compress aggressively.
- **Descriptive Latin filenames**: `satransporto-sqema-magaliti.webp`, not `IMG_2931.webp`.
- **Georgian `alt`** on every meaningful image describing its content; empty `alt=""` for purely decorative ones.
- **Always set explicit `width`/`height`** (or aspect-ratio) to prevent CLS.
- `loading="lazy"` for below-the-fold images; the hero/LCP image gets `fetchpriority="high"` and NO lazy-load.
- Don't ship images larger than their displayed size; serve responsive sizes via `srcset` if used prominently.

---

## 10. Performance / Core Web Vitals (a ranking factor — and your edge)
Targets: **LCP ≤ 2.5s, INP ≤ 200ms, CLS ≤ 0.1** (mobile, field-data). A hand-built static page should hit ~100/100 Lighthouse; competitors won't.
- **CSS:** the page is simple — inline all CSS in a single `<style>` in `<head>` (zero render-blocking external CSS), or one tiny minified `styles.css`. No CSS frameworks/Bootstrap/Tailwind CDN.
- **JS:** keep the checker JS tiny and vanilla; load with `<script defer>` at end of body. No jQuery, no libraries. JS executes after content paints. Heavy/blocking JS kills INP.
- **Fonts (Georgian) — biggest LCP lever:** **self-host** a subsetted Georgian font (e.g. Noto Sans Georgian) as `woff2`, `font-display: swap`, and `<link rel="preload" as="font" type="font/woff2" crossorigin>`. Avoid third-party font CDNs in the critical path; if unavoidable, `preconnect`. Subset to Georgian + Latin + numerals only.
- **No layout shift:** dimensions on all media; reserve space for the checker result card; don't inject above-the-fold content late.
- **Minify** HTML/CSS/JS for production. Enable host compression (Brotli/gzip — automatic on Netlify/Cloudflare). Long cache headers on static assets.
- No third-party embeds in the critical path (no map iframe above the fold; if a map is needed, lazy-load it or link out).

---

## 11. Mobile-first
- `viewport` meta (above). Responsive layout, single-column on mobile.
- Body text ≥16px; Georgian needs comfortable line-height (~1.6).
- Tap targets ≥ 48×48px (the call & quote CTAs especially).
- Test the checker on a narrow viewport — it must be fully usable by thumb.

---

## 12. Conversion-adjacent SEO
- `tel:+995577155575` link on the call CTA (mobile click-to-call + counts as a clear action).
- Quote CTA: `mailto:` or an on-page form; if a form, keep it lightweight and not blocking render.
- Above-the-fold dual CTA improves engagement signals (dwell, low pogo-sticking) which indirectly support ranking.

---

## 13. Measurement & validation (do after deploy)
- **Google Search Console:** verify the subdomain (DNS TXT), submit `sitemap.xml`, then watch Coverage (indexed?), Performance (which Georgian queries/impressions), and Core Web Vitals report.
- **Validate:** Rich Results Test (all JSON-LD), Lighthouse (Performance + SEO ≥95), Mobile-Friendly check, and a JS-off render check (Rule #1).
- **Google Business Profile** in Georgian (category, description, service area, NAP identical to the site).
- Track the real KPI: quote-requests + calls per page, not just rankings.

---

## 14. SEO ACCEPTANCE CHECKLIST (Claude Code must satisfy all before "done")
- [ ] **Rule #1:** with JS disabled, ALL content (8 scheme types, all docs/prices, all FAQs, all body copy) is fully visible in the static HTML.
- [ ] One `<h1>` per page (primary keyword); clean h2/h3 nesting; primary keyword in first 100 words.
- [ ] Exact title tag + meta description per §3; canonical self-referencing; robots index,follow.
- [ ] hreflang `ka` + `x-default` (and reciprocal `en` only if a real equivalent exists).
- [ ] OG + Twitter tags present; `og:locale ka_GE`.
- [ ] JSON-LD: ProfessionalService + Service + FAQPage on each page, all valid in Rich Results Test; FAQ markup matches visible text.
- [ ] Latin-transliterated slugs; no Georgian script in URLs.
- [ ] `sitemap.xml` + `robots.txt`; nothing blocking crawl.
- [ ] Internal links between the two pages (keyword anchors) + link to sts.com.ge; README notes the "add links from sts.com.ge → these pages" action.
- [ ] Images WebP, descriptive Latin filenames, Georgian alt, explicit dimensions, lazy below-fold, hero not lazy.
- [ ] CSS inlined/minified, no framework; checker JS vanilla + deferred; Georgian font self-hosted, subset, preloaded, font-display swap.
- [ ] Lighthouse mobile Performance ≥ 95 and SEO = 100; CLS ≈ 0.
- [ ] Body text ≥16px, tap targets ≥48px, single-column mobile, checker usable on narrow screens.
- [ ] ≥600 words genuine Georgian content per page; 5–8 FAQ items each.
- [ ] NAP in footer as real text; LinkedIn linked.
```
```
