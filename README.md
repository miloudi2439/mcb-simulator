# MCB Recruitment Score Simulator

**محاكي مسابقة توظيف أستاذ مساعد قسم "ب"**

A single-page web app that simulates the official 20-point scoring grid used in Algerian university recruitment competitions for the rank of *Maître de Conférences classe B* / assistant professor class "B" (أستاذ مساعد قسم "ب"). A candidate enters their file, gets an estimated final mark with every per-criterion cap applied, and receives an assessment of where their profile is strong or weak and what to improve first.

The interface is in Arabic (right-to-left).

---

## Features

- **Score calculation across the six criteria** of the ministerial grid, with all sub-caps and global caps enforced.
- **Publication list** where each item has a type (A+, A, B, C, patent, book) and an author position that applies the contribution weighting.
- **Automatic seniority** computed from the doctorate date and the competition opening date, counted in full years.
- **Evaluate button** (`تقييم ملفي`): opens a popup with the final mark and the assessment.
- **Profile assessment**:
  - an overall verdict (very strong / competitive / average / weak);
  - points still gainable through work vs. points lost on fixed criteria;
  - improvement priorities ranked by points gained per unit of effort;
  - a note for each criterion with a rating and concrete advice.
- **Light theme** with a fixed palette, regardless of the system setting.
- **Responsive layout** that works on phones.
- **Private by design**: all calculation happens in the browser and nothing is stored or sent.

## Scoring grid

| # | Criterion | Rule | Max |
|---|-----------|------|-----|
| 1 | Fit of the degree's field (شعبة) and specialty | Division points + specialty points. 1st required division = 1, plus specialty: 1st 1, 2nd 0.75, 3rd 0.5, other 0.25. 2nd required division = 0.75, plus specialty: 1st 0.75, 2nd 0.5, other 0.25 | 2 |
| 2 | Degree grade | Très Honorable = 1; Honorable / equivalent foreign degree = 0.5 | 1 |
| 3 | Degree seniority | 0.25 per full year between the doctorate and the competition opening | 2 |
| 4a | Publications, patents, books | A+ = 5, A / PCT patent = 4, B / INAPI patent = 3, C = 1.5, ISBN book = 1.5; × author weight (1st 100%, 2nd 50%, 3rd+ 25%) | 5 |
| 4b | Conference talks | International 0.5 each (max 2); national 0.5 each (max 1) | 3 |
| 5 | Professional experience | Cours 0.5/semester (max 3); TD 0.25/semester (max 1.5); TP 0.25/year (max 1.5); teaching in other sectors or administrative supervision post 0.5/year (max 1.5) | 3 (overall) |
| 6 | Interview | Analysis & synthesis, clarity of language, communication, scientific skills: 1 point each | 4 |
| | **Total** | | **20** |

### Assumptions

- A degree from a division not listed in the announcement scores 0 on criterion 1.
- The author-position weighting is also applied to books.
- The interview score is the user's own estimate.
- The verdict thresholds (16 / 13 / 10) and the effort levels in the priorities are heuristics, not part of the official grid. The mark actually needed to pass depends on the number of candidates in each specialty.

## Tech stack

| Layer | Technology |
|-------|-----------|
| Markup | HTML5 (`dir="rtl"`, `lang="ar"`) |
| Styling | Plain CSS: custom properties for colors, Grid and Flexbox for layout |
| Logic | Vanilla JavaScript (ES6), no framework or dependencies |
| Fonts | Google Fonts: IBM Plex Sans Arabic, Reem Kufi, IBM Plex Mono |
| Backend | None |

## Getting started

The app is one self-contained file, `index.html`, with no build step.

```bash
# open directly
xdg-open index.html        # Linux
open index.html            # macOS
start index.html           # Windows

# or serve locally
python3 -m http.server 8000
# then visit http://localhost:8000
```

### Deploying

Upload `index.html` to any static host: GitHub Pages, Netlify, Vercel, Cloudflare Pages, or a university web server.

## Files

| File | Purpose |
|------|---------|
| `index.html` | The whole app, plus search metadata (description, canonical URL, Open Graph, JSON-LD `WebApplication` and `FAQPage`) |
| `robots.txt` | Allows all crawlers and points to the sitemap |
| `sitemap.xml` | Lists the page for search engines |
| `og.png` | 1200×630 preview image shown when the link is shared |

The live URL is assumed to be `https://miloudi2439.github.io/mcb-simulator/`. If it changes (for example with a custom domain), update the canonical, `og:url`, `og:image` and JSON-LD URLs in `index.html`, plus `robots.txt` and `sitemap.xml`.

## Code structure

All logic lives in the `<script>` block at the end of `index.html`.

| Function / constant | Role |
|---------------------|------|
| `PUB_TYPES`, `ROLES`, `INTERVIEW`, `CRIT` | Grid configuration: point values, author weights, interview items, criterion caps |
| `fullYears(from, to)` | Full years between two dates (used for seniority) |
| `score(c)` | Computes the six criterion scores and the total from a candidate object, applying all caps |
| `readForm()` / `fillForm(c)` | Converts between the form and a candidate object |
| `renderPubs()` | Draws the editable publication list |
| `assess(c, r)` | Builds the improvement priorities and per-criterion notes |
| `verdictOf(total)` | Maps the total to an overall verdict |
| `update()` | Recomputes everything and refreshes the page on every input |
| `#evalBtn` handler | Recomputes and opens the result popup (`<dialog id="result">`) |

To adapt the grid (for example if the ministry changes point values), edit the constants at the top of the script and the caps inside `score()`.

## Possible extensions

- Arabic/French language toggle.
- Export of the result as a PDF summary.
- Multi-candidate ranking with the official tie-break order (publications → interview → doctorate seniority → age).
- A port to React or Vue with a backend, if accounts or saved profiles are needed.

## Disclaimer

This is an approximate simulation tool based on the grid as documented. Application details may vary between universities; always refer to the official ministerial decree and the selection committee's minutes.

## Author

Amara Miloudi · University of El Oued, Algeria
