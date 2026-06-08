# SEO Roadmap — Robust Decision Making Lab (rdllab.github.io)

> **Goal:** Make the lab website discoverable in Google so that searches like
> *"RDL Lab"*, *"Robust Decision Making Lab"*, and *"RDL Lab ANU"* return
> <https://rdllab.github.io/> on the first page.

> **Status (2026-06-08):** ✅ **Phase 0 and Phase 1 applied** in
> [_config.yml](_config.yml) — except **1.4 (Google Search Console verification)**,
> which is blocked until you generate a verification token in Search Console
> (Phase 2.1). Changes take effect after the next push to `main` rebuilds the site.

## Why the site is currently invisible

The site is built on the [al-folio](README-al-folio.md) Jekyll theme, but several
theme **defaults/placeholders were never replaced** and the search-engine plumbing
is **switched off**. Two things matter most:

1. **The site has almost certainly never been verified in / submitted to Google
   Search Console.** Google does not automatically discover small, low-backlink
   GitHub Pages sites quickly. Until the sitemap is submitted and indexing is
   requested, the site may not be indexed at all.
2. **On-page signals are placeholder text.** The meta description Google would show
   is literally *"A simple, whitespace theme for academics."* — there is nothing
   telling Google this is the *Robust Decision Making Lab*.

Everything below is ordered by impact. **Phase 1 + Phase 2 are what actually fix
"I can't find the site in Google."** The later phases improve ranking and rich
results once it's indexed.

---

## Phase 0 — Diagnose current state (15 min, do first)

> **Findings (checked 2026-06-08):**
> - `site:rdllab.github.io` → the homepage **is already indexed**, but Google
>   displays the old placeholder snippet (*"…simple whitespace academic theme based
>   on \*folio"*). Indexed, but with junk metadata → fixed by Phase 1.1.
> - `"Robust Decision Making Lab" ANU` → the lab site does **not** rank; ANU /
>   [ARIAM Hub](https://ariamhub.com/people/hanna-kurniawati/) profile pages for
>   Prof. Hanna Kurniawati dominate. Needs Phase 2 (request indexing) + Phase 4
>   (backlinks) to climb.
> - `robots.txt` allows all crawlers ✅ — but its `Sitemap:` line was mixed-case
>   `https://RDLLab.github.io/...` (fixed by the lowercase `url` in Phase 1.2).
> - `sitemap.xml` is live and lists correct lowercase `https://rdllab.github.io/`
>   URLs ✅ (the `0.0.0.0:8080` URLs only appear in local dev builds).

- [x] Search Google for `site:rdllab.github.io` — **indexed**, but placeholder snippet.
- [x] Search Google for `"Robust Decision Making Lab"` / `RDL Lab ANU` — site not ranking.
- [x] Confirm the production site is live and crawlable — `robots.txt` allows all ✅.
- [x] Confirm `sitemap.xml` lists real lowercase `https://rdllab.github.io/` URLs ✅.

---

## Phase 1 — Fix on-page SEO foundations (highest impact, ~1 hr)

All changes are in [_config.yml](_config.yml) unless noted. **A config change
requires a rebuild/redeploy to take effect** (push to `main`).

### 1.1 Real site identity

- [x] **Description** ([_config.yml:12-13](_config.yml#L12)) — ✅ **applied.** Now
      describes the RDL Lab, Prof. Hanna Kurniawati, ANU, and the research topics:
      ```yaml
      description: >
        The Robust Decision Making (RDL) Lab at the School of Computing,
        Australian National University (ANU), develops scalable methods for
        sequential decision-making under uncertainty, POMDPs, motion and
        inspection planning, reinforcement learning, and safe autonomy for robots.
      ```
- [x] **Keywords** ([_config.yml:17](_config.yml#L17)) — ✅ **applied:**
      ```yaml
      keywords: RDL Lab, Robust Decision Making Lab, ANU, robotics, POMDP, sequential decision-making under uncertainty, motion planning, reinforcement learning, safe autonomy
      ```
- [x] **Author name** ([_config.yml:7-9](_config.yml#L7)) — ✅ **applied.** Set to the
      lab name, split as `first_name: Robust Decision Making` / `last_name: Lab` so
      `<meta name="author">` renders cleanly as "Robust Decision Making Lab".
- [x] **Scholar author** ([_config.yml:266-267](_config.yml#L266)) — ✅ **applied.**
      Set to the lab's staff (`last_name: [Kurniawati, Kim, Hoerger]` /
      `first_name: [Hanna, Edward, Marcus]`) so their names are highlighted in the
      publication list. *Extend these lists to also bold students/HDR authors.*

### 1.2 Canonical URL casing

- [x] **`url`** ([_config.yml:21](_config.yml#L21)) — ✅ **applied.** Changed
      `https://RDLLab.github.io` → lowercase `https://rdllab.github.io` so `<link rel="canonical">`
      ([_includes/head.liquid:70](_includes/head.liquid#L70)) and sitemap exactly
      match the served (lowercase) host. Avoids any case-variant duplicate-URL
      ambiguity.

### 1.3 Turn on Open Graph + Schema.org

The theme already has full OG/Twitter/JSON-LD support in
[_includes/metadata.liquid](_includes/metadata.liquid) — it's just gated off.

- [x] **`serve_og_meta: true`** ([_config.yml:66](_config.yml#L66)) — ✅ **applied.**
      Now emits `og:title`, `og:description`, `og:url`, `og:image`, Twitter card tags.
- [x] **`serve_schema_org: true`** ([_config.yml:67](_config.yml#L67)) — ✅ **applied.**
      Now emits the `application/ld+json` block.
- [x] **`og_image`** ([_config.yml:68](_config.yml#L68)) — ✅ **applied:** set to
      `https://rdllab.github.io/assets/img/rdl_logo.png`. *Optional later: replace with
      a dedicated ~1200×630 banner — logos can look cropped in social previews.*

### 1.4 Set up Google Search Console verification (config side)

> ⏳ **Blocked — needs a token from Search Console (do Phase 2.1 first).** Left the
> toggle `false` for now, because enabling it with an empty token would emit a
> useless empty `<meta>` tag. Once you have the token, set both fields and flip the
> toggle to `true`, then redeploy.

- [ ] **`enable_google_verification: true`** ([_config.yml:382](_config.yml#L382)).
- [ ] **`google_site_verification`** ([_config.yml:82](_config.yml#L82)) — paste the
      verification token from Search Console (see Phase 2.1). This makes
      [_includes/metadata.liquid:4](_includes/metadata.liquid#L4) emit the
      `<meta name="google-site-verification">` tag.
- [ ] *(Optional)* do the same for Bing:
      `enable_bing_verification: true` + `bing_site_verification`.

### 1.5 Quick bug fix

- [x] Fix the typo in `contact_note` ([_config.yml:11](_config.yml#L11)):
      `hred=` → `href=` — ✅ **applied** (the People-page link in the contact note now works).

---

## Phase 2 — Get indexed (highest impact, ~30 min + waiting)

This is the step that most directly answers *"why can't I find it on Google."*

> **Sitemap note:** Submit the **plugin-generated** sitemap at
> `https://rdllab.github.io/sitemap.xml` ([jekyll-sitemap](_config.yml#L214) builds it
> automatically). **Do not commit a hand-made static `sitemap.xml`** — a source file
> by that name makes the plugin skip generation, replacing the auto sitemap with a
> stale one (a manual xml-sitemaps.com file was created and removed for this reason).

> **Prerequisite:** the Phase 1 edits (+ the verification token below) must be
> **committed, pushed, and deployed** before GSC verification will succeed.

### 2.1 Google Search Console

- [ ] Go to <https://search.google.com/search-console>, sign in with the lab Google
      account.
- [ ] Add a **URL-prefix property** for `https://rdllab.github.io/`.
- [ ] Choose the **HTML tag** verification method, copy the `content` token, and put
      it into `google_site_verification` (Phase 1.4). Push/deploy, then click
      **Verify**.
      - *Alternative:* the "HTML file" method also works on GitHub Pages — drop the
        provided file at the repo root so it's served at the site root.
- [ ] In Search Console → **Sitemaps**, submit `sitemap.xml`
      (full: `https://rdllab.github.io/sitemap.xml`). The
      [jekyll-sitemap](_config.yml#L211) plugin already generates it.
- [ ] Use **URL Inspection** on `https://rdllab.github.io/` → **Request indexing**.
      Repeat for key pages: `/people/`, `/publications/`, `/project/`, `/software/`,
      `/news/`.

### 2.2 Verify nothing blocks crawling

- [ ] Confirm production [robots.txt](robots.txt) allows all (it does) and that the
      `Sitemap:` line resolves to `https://rdllab.github.io/sitemap.xml` after the
      `url` casing fix.
- [ ] Confirm no page sets `<meta name="robots" content="noindex">` (none currently do).

> **Expectation:** after verification + sitemap submission + "Request indexing,"
> the homepage typically gets indexed within a few days to ~2 weeks. Ranking #1 for
> the brand name follows once indexed (Phase 3/4 reinforce it).

---

## Phase 3 — On-page content optimization (medium impact, ongoing)

### 3.1 Make the brand name unmistakable on the homepage

- [ ] The homepage ([_pages/home.md](_pages/home.md)) sets `show_title: false` and the
      opening paragraph starts "Our lab focuses on…". Add an explicit, crawlable
      mention of **"Robust Decision Making (RDL) Lab"** and **"School of Computing,
      Australian National University"** in the first sentence / a visible heading so
      both the full name and the acronym appear in body text and the `<h1>`/`<title>`.
- [ ] Confirm the rendered `<title>` is `Robust Decision Making Lab` (driven by
      `title:` in [_config.yml:5](_config.yml#L5) via
      [_includes/metadata.liquid:23-40](_includes/metadata.liquid#L23)) — good as-is.

### 3.2 Per-page descriptions

- [ ] Add a unique, keyword-rich `description:` to each page's front matter. Several
      are empty or still placeholders:
  - [ ] [_pages/contact.md](_pages/contact.md) — `description:` is empty.
  - [ ] [_pages/cv.md](_pages/cv.md) — placeholder *"This is a description of the page…"*.
  - [ ] [_pages/projects.md](_pages/projects.md) — placeholder *"A growing collection of your cool projects."*.
  - [ ] [_pages/people.md](_pages/people.md), [_pages/project.md](_pages/project.md),
        [_pages/publications.md](_pages/publications.md),
        [_pages/software.md](_pages/software.md) — tighten to ~150 chars, each
        mentioning the lab + the page topic.

### 3.3 Images & accessibility (also an SEO signal)

- [ ] Ensure every image has descriptive `alt` text (lab photos, member photos,
      project figures). The theme already does WebP + lazy-loading
      ([_config.yml:349-372](_config.yml#L349)).
- [ ] Set the homepage `profile.image` ([_pages/home.md:10](_pages/home.md#L10)) — it's
      empty; a lab logo/photo here improves the page and gives an indexable image.

### 3.4 Headings & internal links

- [ ] Use a clear `<h1>` → `<h2>` hierarchy on each page (one `<h1>` per page).
- [ ] Cross-link pages with descriptive anchor text (e.g. "see our
      [publications](/publications/)") rather than bare URLs. The homepage already
      links to the project page — extend this.

---

## Phase 4 — Off-page / authority (medium impact, the real long-term lever)

A brand-name search ranks largely on **who links to you**. For a new lab site this
is the difference between "indexed but buried" and "first result."

- [ ] **ANU School of Computing** — get the official lab/group page and PI staff
      profile to link to <https://rdllab.github.io/>. This is the single most
      valuable backlink (high-authority `.edu.au` domain).
- [ ] **Lab members' pages** — each member's personal/ANU/LinkedIn page links to the
      lab site.
- [ ] **Google Scholar** — add the lab website URL to the PI's and members' Scholar
      profiles; ensure publications list the lab.
- [ ] **GitHub** — add the website URL to the [RDLLab GitHub org](https://github.com/RDLLab)
      profile and to relevant repo "About" / README sections.
- [ ] **Software/ROS package pages, dataset pages, paper "project pages"** — link back
      to the lab site.
- [ ] **Conference/workshop bios, co-author institution pages** — link where possible.

---

## Phase 5 — Richer structured data (lower impact, nice-to-have)

Once `serve_schema_org` is on (Phase 1.3), the JSON-LD emitted is a generic
`WebSite`/`Person`. To get an entity Google recognizes as a research organization:

- [ ] Add the social/identity fields used by
      [_includes/metadata.liquid:82-218](_includes/metadata.liquid#L82) (e.g.
      `github_username`, `scholar_userid`, `linkedin_username`) so a `sameAs` array
      links the site to authoritative profiles — strengthens entity disambiguation.
- [ ] *(Advanced)* Consider a custom `Organization` / `ResearchOrganization` JSON-LD
      block for the homepage with `name`, `alternateName: "RDL Lab"`, `url`,
      `parentOrganization` (ANU), and `logo`. This can be added to
      [_includes/metadata.liquid](_includes/metadata.liquid) or via a homepage include.
- [ ] Publications already get scholarly metadata via jekyll-scholar; verify the
      rendered publication pages expose author/title/year cleanly.

---

## Phase 6 — Performance & technical health (lower impact, mostly already good)

The theme ships strong defaults (minification, Terser, compressed Sass, WebP, lazy
loading). Still worth confirming:

- [ ] Run **Google PageSpeed Insights / Lighthouse** on the homepage and check Core
      Web Vitals (LCP, CLS, INP). The repo even has a
      [lighthouse-badger workflow](.github/workflows/lighthouse-badger.yml) and
      [lighthouse_results/](lighthouse_results/) — review the latest scores.
- [ ] Confirm there are no crawl errors / broken links — repo has
      [broken-links workflows](.github/workflows/broken-links.yml); review output.
- [ ] Ensure HTTPS is enforced (GitHub Pages does this for `*.github.io`).

---

## Phase 7 — Analytics & monitoring (ongoing)

- [ ] **Enable Google Analytics** — a GA4 ID `G-WNPSY5ZZN0` is already set
      ([_config.yml:76](_config.yml#L76)) but `enable_google_analytics: false`
      ([_config.yml:378](_config.yml#L378)). Flip to `true` to start collecting
      traffic data (confirm the ID belongs to the lab).
- [ ] **Monitor Search Console** weekly for the first month: Coverage/Pages
      (indexed count), Performance (impressions/clicks for "RDL Lab" etc.), and any
      Enhancements/structured-data errors.
- [ ] Re-run `site:rdllab.github.io` after ~1–2 weeks to confirm indexing growth.

---

## Quick-win checklist (do these first)

The minimum set that fixes "I can't find RDL Lab in Google":

1. [x] Fix `description` + `keywords` + `author` + `url` casing ([_config.yml](_config.yml)) — ✅ **done.**
2. [x] `serve_og_meta: true`, `serve_schema_org: true`, set `og_image` — ✅ **done.**
3. [ ] Verify the site in **Google Search Console** (`enable_google_verification: true`
       + token). ← **next action for you** (Phase 2.1).
4. [ ] **Submit `sitemap.xml`** and **Request indexing** of the homepage in Search Console.
5. [ ] Get **ANU School of Computing** and **lab members** to link to
       <https://rdllab.github.io/>.

> Steps 1–4 are code/config + a few clicks and can be done today. Step 5 is the
> durable ranking lever. After deploying 1–2 and verifying 3–4, indexing usually
> follows within days.
