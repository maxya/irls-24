# 2026 SEO & Growth Strategy Guide for iRLS (Irina Beckel)

This document is both a strategy and an implementation checklist tailored to this repository (Pelican static site, Markdown content under `content/`, build output under `output/`, CI deploy via GitHub Pages).

## Executive Summary

- **Niche**: Professional Russian Language Services (tutoring + translation/interpretation).
- **Core identity**: Personalized, high‑touch service by an expert native speaker.
- **Primary goal**: Move from a passive “online business card” to a **lead generation engine**.

## Repo Reality Check (How this site is built)

- **Content source**: `content/` (home/article content) and `content/pages/` (static pages).
- **Bilingual pattern**: paired files like `about.md` and `about-ru.md` with `lang: en|ru` and matching `Slug:`.
- **Build commands**:
  - `make html` (build)
  - `make devserver` (serve + autoreload)
  - `make clean && make html` (clean build)
  - `make setup` (create venv + install deps via `uv`)

## Implementation Plan (Phased, with checkboxes)

### Phase 0 — Baseline, measurement, and safe rollout (1–2 hours)

- [ ] Run `make clean && make html` and confirm the site renders locally.
- [ ] Decide **one primary conversion action** to optimize for:
  - tutoring leads (discovery call), or
  - translation quote requests.
- [ ] Pick tracking approach (lightweight and privacy‑friendly):
  - [ ] GA4, or
  - [ ] Plausible, or
  - [ ] no JS analytics (server logs only).

Acceptance criteria:
- Local build works.
- A single primary CTA is chosen.

---

### Phase 1 — Layout & UX Optimization (Retention‑First) (0.5–2 days)

#### 1) Above‑the‑fold reconstruction (homepage)

- [ ] Update the homepage content to lead with a benefit‑driven H1, a clear sub‑headline, and one CTA.
- [ ] Add 2–3 trust signals near the CTA (credentials, years, domains served, local presence).

Suggested content structure for the homepage Markdown (example skeleton):

```md
# Master Russian with a Native Expert

Short, specific promise: business / travel / exam prep / translation support.

**Primary CTA:** Request a quote / Book a discovery call

## Services
- Private Russian tutoring (online)
- English ⇄ Russian translation
- Court / medical interpreting (as applicable)

## Why iRLS
- 15+ years experience
- Native speaker (RU), fluent EN
- Located in Maryland Heights, MO (if you want local SEO)
```

#### 2) Bilingual toggle switch (sitewide)

Goal: a prominent, consistent language switcher that uses Pelican’s translation objects.

- [ ] Add a language switcher in the main nav (sticky if theme supports it).
- [ ] Ensure each EN page links to RU translation and vice‑versa.

Template snippet (Jinja2) that works in Pelican themes **when pages/articles have translations**:

```jinja2
{# In a nav/header template #}
<nav class="lang-switch" aria-label="Language">
  {% if page is defined and page.translations %}
    <a href="/{{ page.url }}" hreflang="{{ page.lang }}" aria-current="page">{{ page.lang|upper }}</a>
    {% for t in page.translations %}
      <a href="/{{ t.url }}" hreflang="{{ t.lang }}">{{ t.lang|upper }}</a>
    {% endfor %}
  {% elif article is defined and article.translations %}
    <a href="/{{ article.url }}" hreflang="{{ article.lang }}" aria-current="page">{{ article.lang|upper }}</a>
    {% for t in article.translations %}
      <a href="/{{ t.url }}" hreflang="{{ t.lang }}">{{ t.lang|upper }}</a>
    {% endfor %}
  {% endif %}
</nav>
```

Notes:
- If the current theme does not expose `page.translations` / `article.translations` as expected, fall back to manual links in the Markdown pages (simple but reliable).

#### 3) “Book a discovery call” widget (high‑intent lead capture)

- [ ] Choose provider (Cal.com or Calendly).
- [ ] Embed on homepage and/or “About” page.

Example embed (Calendly-style):

```html
<div class="cal-embed">
  <iframe
    src="https://cal.com/your-handle/your-event"
    style="width:100%;height:760px;border:0"
    loading="lazy"
  ></iframe>
</div>
```

#### 4) Interactive proficiency quiz (2–3 minute “meaningful interaction”)

- [ ] Add a small quiz (10 questions) with a results summary + CTA.
- [ ] Keep JS tiny and framework-free.

Minimal inline example (works inside a Markdown page in Pelican):

```html
<section id="ru-quiz">
  <h2>Test your Russian level (2 minutes)</h2>
  <button id="quiz-start">Start</button>
  <p id="quiz-result" hidden></p>
</section>

<script>
  const start = document.getElementById('quiz-start');
  const result = document.getElementById('quiz-result');
  start.addEventListener('click', () => {
    // Replace with real quiz flow later
    result.hidden = false;
    result.textContent = 'Result: A2–B1. Want a tailored plan? Book a discovery call.';
  });
</script>
```

Acceptance criteria:
- Homepage has a single primary CTA and is visually scannable.
- Language switcher is always visible and correct.
- Quiz is usable on mobile.

---

### Phase 2 — Keyword Matrix & Semantic SEO (0.5–1 day)

#### Keyword strategy (turn table into implementation)

- [ ] Map each keyword group to an actual URL/page in this repo.
- [ ] Decide which pages must exist in **both EN + RU**.

Recommended “power pages” (service intent):

- [ ] `services-russian-tutoring` (EN/RU)
- [ ] `services-translation` (EN/RU)
- [ ] `services-interpreting` (EN/RU)
- [ ] `russian-for-business` (EN/RU)
- [ ] `faq` (EN/RU)

#### AI Overview (SGE) optimization pattern

- [ ] Add Q&A sections to high‑intent pages.
- [ ] Use direct-answer formatting immediately under headings.

Example:

```md
## How much does a Russian tutor cost?
Most professional 1:1 tutoring ranges from $X–$Y/hour depending on goals (conversation, business, exam prep) and lesson frequency.
```

---

### Phase 3 — Content & E‑E‑A‑T upgrades (1–3 days)

Goal: rewrite content from descriptive → evidential, with proof and specificity.

- [ ] Update each top page with:
  - [ ] 1–3 concrete outcomes (exam passed, turnaround times, industries served)
  - [ ] process description (how you work)
  - [ ] clear CTA
- [ ] Add “Experience-led” statements (what you’ve learned in practice).

Markdown metadata improvements (Pelican-friendly):

- [ ] Add `Summary:` to key pages to improve snippets and internal listings.

Example refactor header:

```md
Title: About Irina Beckel — Russian Tutor & Translator
Slug: about
lang: en
Summary: Native Russian speaker with 15+ years teaching and translation experience.
```

---

### Phase 4 — Technical SEO & Schema injection (1–2 days)

#### 1) Canonical + hreflang (template-level)

- [ ] Add a canonical tag in the base template.
- [ ] Ensure language alternates (hreflang) exist for translated pages.

Canonical example (Jinja2):

```jinja2
{% if page is defined %}
  <link rel="canonical" href="{{ SITEURL }}/{{ page.url }}">
{% elif article is defined %}
  <link rel="canonical" href="{{ SITEURL }}/{{ article.url }}">
{% endif %}
```

hreflang example (pairs current object + translations):

```jinja2
{% set obj = page if page is defined else article %}
{% if obj is defined %}
  <link rel="alternate" hreflang="{{ obj.lang }}" href="{{ SITEURL }}/{{ obj.url }}">
  {% for t in obj.translations %}
    <link rel="alternate" hreflang="{{ t.lang }}" href="{{ SITEURL }}/{{ t.url }}">
  {% endfor %}
{% endif %}
```

#### 2) JSON-LD schema (sitewide)

- [ ] Add JSON‑LD in `<head>` so every page can be cited by AI systems.
- [ ] Keep `@graph` stable; update `sameAs` only when real profiles exist.

Recommended baseline schema (adjust links/handles):

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@graph": [
    {
      "@type": "Person",
      "@id": "https://irlsonline.com/#irina",
      "name": "Irina Beckel",
      "jobTitle": "Russian Language Tutor & Translator",
      "url": "https://irlsonline.com",
      "knowsLanguage": ["en", "ru"],
      "worksFor": { "@id": "https://irlsonline.com/#org" }
    },
    {
      "@type": "LocalBusiness",
      "@id": "https://irlsonline.com/#org",
      "name": "iRLS (Russian Language Services)",
      "url": "https://irlsonline.com",
      "telephone": "+1-314-630-4636",
      "address": {
        "@type": "PostalAddress",
        "streetAddress": "11650 Dorsett Rd",
        "addressLocality": "Maryland Heights",
        "addressRegion": "MO",
        "postalCode": "63043",
        "addressCountry": "US"
      },
      "areaServed": ["US", "Worldwide"],
      "knowsLanguage": ["en", "ru", "uk"],
      "priceRange": "$$"
    },
    {
      "@type": "Service",
      "name": "Private Russian Tutoring",
      "provider": { "@id": "https://irlsonline.com/#org" },
      "serviceType": "Language Instruction",
      "areaServed": "Worldwide"
    },
    {
      "@type": "Service",
      "name": "Russian Translation Services",
      "provider": { "@id": "https://irlsonline.com/#org" },
      "serviceType": "Translation",
      "areaServed": "Worldwide"
    }
  ]
}
</script>
```

#### 3) Core Web Vitals / media

- [ ] Convert `Selfpresenation2.jpg` to WebP/AVIF and update references.

Example macOS conversion (WebP via `cwebp` if installed):

```bash
cwebp content/images/Selfpresenation2.jpg -q 80 -o content/images/Selfpresenation2.webp
```

If you prefer no new tooling, keep images but ensure width/height in templates to reduce CLS.

---

### Phase 5 — Competitor Conquest (ongoing, 2–4 weeks)

- [ ] Produce “10x” content competitors can’t replicate:
  - [ ] industry-specific Russian negotiation vocabulary
  - [ ] annotated literature analysis for English speakers
  - [ ] audio pronunciations (short clips)
- [ ] Add downloadable assets (PDF cheat sheets) + email capture if desired.

## Next Steps (Immediate Action Plan)

1. [ ] Implement schema + canonical/hreflang in templates.
2. [ ] Rewrite homepage (`content/irls.md`) into a conversion-focused landing page.
3. [ ] Add an always-visible language switcher.
4. [ ] Add one “pillar” article targeting a high-intent question.

## Notes / Open Decisions

- Do you want to position primarily as **tutoring** or **translation/interpreting** on the homepage? (This changes the CTA, schema emphasis, and page hierarchy.)
- Are there real profiles to link in `sameAs` (LinkedIn, Google Business Profile, etc.)? If not, omit them (better than placeholders).
