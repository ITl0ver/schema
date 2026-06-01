# Build Spec — STS Georgian Service Pages (self-hosted, static)

**For:** Claude Code (run in Terminal inside an empty git repo)
**Deliverable:** Two self-contained static pages in Georgian, deployable to a static host (Netlify / Cloudflare Pages / GitHub Pages) and attached to a `sts.com.ge` **subdomain**. These are SEO assets — Google Ads doesn't support Georgian, but organic Search does.

---

## 0. Read first / guardrails
1. Build **plain static files** — HTML + CSS + vanilla JS. No framework, no build step. One folder, one page per file, plus a shared CSS file. Keep it simple enough to edit by hand and `git push`.
2. Match the existing STS brand from https://sts.com.ge/ — clean, professional, B2B. Pull logo, fonts, and color feel from there (dark logo on light background; the site uses a muted gold/tan accent ~`rgb(213,199,136)`). Don't copy their code; just keep visual continuity.
3. **Georgian only.** All visible text in Georgian. Use a Georgian-friendly web font (e.g. Noto Sans Georgian) so glyphs render consistently.
4. Use the body copy in the companion file `STS_Georgian_LandingPages_and_Ads.md` verbatim for the TIA page and the scheme page prose. Do not rewrite or re-translate.
5. **SEO is the priority.** Read `STS_SEO_Technical_Spec.md` — it is the authoritative source on ALL SEO matters and overrides this file wherever they conflict. The build must pass its acceptance checklist.
6. **Do not fabricate** prices, phone, email, or legal thresholds beyond what's given here. Where a value is unknown leave a clear `[ ... ]` placeholder. Known contacts: phone `+995 599 120 755`, email `info@sts.com.ge`, address `3/17 Ana Politkovskaia St, Tbilisi 0168`.
7. Propose the file structure first, then build.

---

## 1. Pages & slugs

| Page | File | Purpose |
|---|---|---|
| TIA | `transportis-zemoqmedebis-shefaseba.html` (or `index` of the TIA subdomain) | Conversion landing page, no checker |
| Scheme | `sagzao-skema.html` | Conversion page **+ interactive checker + static guide** |
| Shared styles | `styles.css` | One stylesheet for both |

Hosting: each page on a subdomain (e.g. `tia.sts.com.ge`, `sqema.sts.com.ge`) or one subdomain with both pages. **No subdomain SEO penalty worth worrying about for low-competition terms — but never a brand-new separate domain.**

---

## 2. TIA page — section order
1. Hero: H1 `ტრანსპორტის ზემოქმედების შეფასება (TIA)`, one-line promise, **two CTAs side by side**: „შეთავაზების მიღება" (scrolls to quote form / mailto) and „დარეკვა" (tel: link). Both CTAs carry equal weight.
2. „როდის გჭირდებათ?" — when a TIA is legally required.
3. „რას მოიცავს" — what's included.
4. „რატომ STS" — ADB/GIZ experience, PTV VISSIM/VISUM/ArcGIS, fast + affordable.
5. Process strip (4 steps).
6. FAQ (accordion) — with FAQPage JSON-LD.
7. Footer CTA repeated (call + quote) + contacts.

Use synonym variants as H2/H3 (ტრანსპორტზე ზემოქმედების შეფასება, ტრანსპორტის ზეგავლენის შეფასება, სატრანსპორტო კვლევა).

---

## 3. Scheme page — section order
1. Hero: H1 `საგზაო მოძრაობის ორგანიზების სქემა — დროებითი და მუდმივი`, one-line promise, dual CTA (quote + call).
2. „როდის გჭირდებათ?" — temporary vs permanent explainer.
3. **INTERACTIVE CHECKER** (see §4) — the centerpiece.
4. **STATIC GUIDE** — the same 8 types as an accordion, so the page is fully readable and indexable with JS off (SEO + fallback). Build this from the same data object in §5.
5. „რატომ STS" — proven experience (annual contracts with major utility/infrastructure companies), fast + affordable, approval-ready docs.
6. FAQ (accordion) + FAQPage JSON-LD.
7. Footer CTA (call + quote) + contacts.

---

## 4. The checker — behavior
- Single question to start: **„რომელი სქემა გჭირდებათ?"** → a `<select>` or button grid with the 8 scheme types (labels in §5).
- On selection, render a result card showing, for that type:
  - **ტიპი:** temporary (დროებითი) or permanent (მუდმივი) — from data.
  - **ვინ ამტკიცებს:** approval goes through City Hall (მერია) and Patrol Police (საპატრულო პოლიცია). (Generic line; STS prepares & submits.)
  - **საჭირო მასალები:** the document checklist for that type (required vs optional clearly marked).
  - **ვადა და ფასი:** the pricing/turnaround tiers for that type.
  - **CTA:** „ამ დოკუმენტებს ჩვენ მოვამზადებთ" → dual button: „მიიღეთ შეთავაზება" + „დარეკვა".
- **Anonymous** — the checker collects NO personal data (no name/ID/cadastral). Those live in the Google Form, which is the downstream intake after the visitor calls or requests a quote.
- Pure client-side, data-driven from a single JS object (§5). No backend, no storage APIs.
- Accessible: keyboard-navigable, the static guide (§3.4) duplicates all content for no-JS users.

---

## 5. Checker data model (extracted from the live form — source of truth)

Build a single JS object `SCHEMES`. All 8 types below. `required: true` = mandatory document; `required: false` = optional/if-available.

**Pricing models referenced below:**
- `PERMANENT`: მუდმივი ან მუდმივი+დროებითი სქემა — `3 დღეში – 1000₾ +18% დღგ`, `10 დღეში – 500₾ +18% დღგ`, `1 თვეში – უფასო`.
- `FENCE_TEMP` (>1 კვირა): `1 დღეში – 700₾`, `5 დღეში – 500₾`, `10 დღეში – 300₾`, `1 თვეში – უფასო` (+18% დღგ).
- `TEMP_TIERED` (three duration bands):
  - 12 საათამდე: `1 დღე – 300₾ / 5 დღე – 150₾ / 10 დღე – 100₾ / 1 თვე – უფასო`
  - 1 კვირამდე (7 დღე): `1 დღე – 500₾ / 5 დღე – 300₾ / 10 დღე – 150₾ / 1 თვე – უფასო`
  - 1 კვირაზე მეტი: `1 დღე – 700₾ / 5 დღე – 500₾ / 10 დღე – 300₾ / 1 თვე – უფასო` (all +18% დღგ)
- Expedited note (show as small print under any tier): დაჩქარებული ატვირთვისას ემატება მერიის/საპატრულო პოლიციის ატვირთვის საფასური +18% დღგ + ბანკის საკომისიო.

### 1. მრავალბინიანი პროექტის შეთანხმება — `type: მუდმივი`, pricing `PERMANENT`
- მშენებლობის პროექტი (ე.წ. მოპ), DWG — **required**
- ტერიტორიის დეტალური გეგმა (dwg, pdf) — 68-127 დადგენილების ცვლილებით, ტროტუარები შიდა გეგმარებაში და საკადასტროს მიმდებარედ — **required**
- საკადასტროს მიმდებარედ ტოპოგრაფიული გეგმა (DWG) — **required**
- დენდროლოგის ფაილი — optional (არსებობის შემთხვევაში)
- პარკირების რაოდენობა — ტექსტი: რამდენი მიწისქვეშ / რამდენი მიწის ზემოთ — **required (text)**
- არსებული სიტუაციის ფოტო/ვიდეო + 3D რენდერი — **required**

### 2. ინდივიდუალური სახლის პროექტის შეთანხმება — `type: მუდმივი`, pricing `PERMANENT`
- მშენებლობის პროექტი (მოპ), DWG — optional
- ტერიტორიის დეტალური გეგმა (dwg, pdf) — 68-127 დადგენილებით, ტროტუარები — **required**
- საკადასტროს ტოპოგრაფიული გეგმა (DWG) — **required**
- დენდროლოგის ფაილი — optional
- არსებული სიტუაციის ფოტო/ვიდეო + 3D რენდერი — optional
- (no parking question)

### 3. სამშენებლო ღობის მოწყობის სქემა — `type: დროებითი`, pricing `FENCE_TEMP`
- მშენებლობის პროექტი (მოპ), DWG — **required**
- საპროექტო ტერიტორიის ტოპო — **required**
- არსებული სიტუაციის ფოტო/ვიდეო + 3D რენდერი — **required**
- ⚠️ NOTE: the form has an ADDITIONAL sub-section after this one (gated behind required upload, not captured). Nikoloz to supply its fields.

### 4. გაპის ეტაპი — `type: მუდმივი`, pricing `PERMANENT`
- საპროექტო ტერიტორიის გენგეგმა და ტოპო, სამანქანო შესასვლელის მითითებით — **required**
- არსებული სიტუაციის ფოტო/ვიდეო + 3D რენდერი — **required**

### 5. ტექნიკის განთავსება/გადაკეტვა — `type: დროებითი`, pricing `TEMP_TIERED`
- ტექნიკის ლოკაცია — ნახაზზე სად დგება ტექნიკა და მისი გაბარიტები (1 ფაილი, max 10MB) — **required**
- რა ვადით განთავსდება ტექნიკა — ტექსტი (მაგ. კონკრეტული თარიღები/სიხშირე) — **required (text)**
- არსებული სიტუაციის ფოტო/ვიდეო + 3D რენდერი — **required**

### 6. გრგ, მრავალფუნქციური კომპლექსი — `type: მუდმივი`, pricing `PERMANENT`
- გრგს დეტალური გენგეგმა (dwg, pdf) — 68-127 დადგენილებით, ტროტუარები შიდა გეგმარებაში და საკადასტროს მიმდებარედ — **required**
- გრგ-ს არეალის ტოპოგრაფიული გეგმა (DWG) — optional (არსებობის შემთხვევაში)
- არსებული სიტუაციის ფოტო/ვიდეო + 3D რენდერი — **required**

### 7. მიწის ტრანშეის გაჭრითი სამუშაოები — `type: დროებითი`, pricing `TEMP_TIERED`
- ტრანშეის გაჭრის ადგილმდებარეობა — ნახაზზე სად, რა სიგანე/სიგრძე, რა არეალი (1 ფაილი, max 10MB) — optional
- არსებული სიტუაციის ფოტო/ვიდეო + 3D რენდერი — **required**

### 8. სხვადასხვა ტიპის კომუნიკაციები — `type: დროებითი`, pricing `TEMP_TIERED`
(ელ.ენერგია, ბუნებრივი აირი, წყალი, ინტერნეტი)
- კომუნიკაციის პროექტი (up to 5 ფაილი) — **required**
- ⚠️ NOTE: additional sub-section after this one (gated behind required upload, not captured). Nikoloz to supply.

> The checker presents these as a "what you'll need to prepare" guide. It does NOT replicate the file-upload intake — that's the Google Form, reached via the CTA.

---

## 6. SEO (both pages — set directly in the HTML)
- `<html lang="ka">`.
- Per-page title tag and meta description (authoritative copy in `STS_SEO_Technical_Spec.md` §3):
  - TIA: `ტრანსპორტის ზემოქმედების შეფასება — სწრაფად | STS`
  - Scheme: `საგზაო მოძრაობის ორგანიზების სქემა — დროებითი/მუდმივი | STS`
- One H1 per page; variant terms as H2/H3.
- hreflang tags (`ka`, and `en` pointing to the relevant sts.com.ge page, `x-default`).
- FAQPage JSON-LD on both; Organization/ProfessionalService JSON-LD sitewide (STS, logo, phone, email, address, areaServed=Georgia/Tbilisi).
- `sitemap.xml` + `robots.txt`; pages indexable.
- Georgian alt text on images; fast, mobile-first, no layout shift.
- Internal links: Georgian TIA page ⇄ scheme page, and link back to sts.com.ge.

---

## 7. Deploy (document in a short README the repo)
1. `git init`, commit the files.
2. Connect repo to Netlify / Cloudflare Pages / GitHub Pages (auto-deploy on push).
3. Add subdomain: CNAME `tia` (and/or `sqema`) → host target. (One-time DNS step via whoever controls sts.com.ge DNS.)
4. After live: submit sitemap in Google Search Console; create/claim a Georgian Google Business Profile.

---

## 8. Acceptance criteria
- [ ] Two static pages, all Georgian, copy verbatim from the copy file.
- [ ] Dual call + quote CTA on both, above the fold and repeated in footer.
- [ ] Scheme page checker: 8 types, each shows type/approver/required materials/pricing + CTA; anonymous (no personal data).
- [ ] Static accordion guide duplicates all 8 types for no-JS / SEO.
- [ ] Title tag, meta description, single H1, variant H2s, hreflang, FAQ + Organization JSON-LD — per §6.
- [ ] sitemap.xml + robots.txt; pages indexable; internal links present.
- [ ] No framework, no build step, no browser-storage APIs; vanilla HTML/CSS/JS only.
- [ ] Placeholders (`[price detail]`, etc.) left intact, not fabricated.
- [ ] README documents deploy + DNS + Search Console steps.

## 9. Open items for Nikoloz to fill before/after build
- Standard turnaround/price line for the TIA service (the scheme prices are embedded above; TIA price is `[ ]`).
- The two unreachable sub-sections (types 3 and 8).
- Confirm subdomain name(s) for the CNAME.
