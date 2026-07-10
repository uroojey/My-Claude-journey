# 📄 PDF Splitter & Merger

A premium, single-page, **100% client-side** tool for splitting and merging PDF files — no uploads, no backend, no accounts. Every operation runs inside the browser tab you already have open.

![type](https://img.shields.io/badge/type-single--file%20HTML%20app-2B6CB0)
![backend](https://img.shields.io/badge/backend-none-2F8F5B)
![license](https://img.shields.io/badge/license-MIT-8B9598)

---

## Table of Contents

- [Why this exists](#why-this-exists)
- [Features](#features)
- [Tech stack](#tech-stack)
- [Getting started](#getting-started)
- [Repo structure](#repo-structure)
- [Processed PDF examples](#processed-pdf-examples)
- [Key learnings](#key-learnings)
- [Accessibility](#accessibility)
- [Roadmap ideas](#roadmap-ideas)
- [License](#license)

---

## Why this exists

Most free online PDF tools require uploading your document to a third-party server before you get anything back. This project does the opposite: it's a single HTML file that loads a couple of libraries from a CDN once, then does all page rendering, splitting, and merging **in memory, in your browser**. Nothing is ever transmitted anywhere. It also works offline once the page has loaded.

## Features

### ✂️ Split
- Auto-detects total page count on upload
- Real visual thumbnails for every page, lazily rendered as you scroll
- Four splitting modes:
  | Mode | What it does |
  |---|---|
  | **Custom ranges** | Define multiple page ranges — each becomes its own output file |
  | **Split after pages** | Click a scissors icon between two pages to cut there |
  | **Split every N pages** | Slider-driven even chunking |
  | **Extract pages** | Click to select pages → combine into one file or export one-per-page |
- Live preview of the resulting file structure before processing
- Inline validation with clearly highlighted invalid ranges
- Per-file downloads or one-click "download all as ZIP"

### 🔗 Merge
- Drag-and-drop or multi-select file picker
- Real first-page thumbnail + page count per file
- Reorder via drag-and-drop or arrow keys
- Live stats: file count, total pages, estimated output size
- Editable output filename before downloading

### Everywhere else
- 🌗 Dark mode
- ⌨️ Keyboard shortcuts (`1` / `2` to switch tools, `Enter` to run, `?` for help)
- 📶 Works fully offline after first load
- 🧯 Clear inline error handling — no silent failures
- ♿ Accessible by default (see [Accessibility](#accessibility))
- 📱 Responsive down to mobile

## Tech stack

| Purpose | Library | Notes |
|---|---|---|
| Rendering page thumbnails | [`pdf.js`](https://mozilla.github.io/pdf.js/) | Renders each page to `<canvas>` |
| Splitting / merging PDFs | [`pdf-lib`](https://pdf-lib.js.org/) | Reads page counts, copies pages, builds new PDF byte arrays |
| ZIP export | [`JSZip`](https://stuk.github.io/jszip/) | Bundles multi-file split output into one download |

All three are loaded from `cdnjs.cloudflare.com` via `<script>` tags — there is no build step, no `package.json`, and no bundler. It's plain HTML/CSS/JS in one file.

## Getting started

No installation needed.

```bash
# Clone the repo
git clone https://github.com/your-username/pdf-splitter-merger.git
cd pdf-splitter-merger

# Just open it — no server, no build step
open pdf-splitter-merger.html      # macOS
xdg-open pdf-splitter-merger.html  # Linux
start pdf-splitter-merger.html     # Windows
```

Or host it anywhere that serves static files (GitHub Pages, Netlify, S3, etc.) — it's a single file.

## Repo structure

```
pdf-splitter-merger/
├── pdf-splitter-merger.html              # the application (HTML + CSS + JS, self-contained)
├── README.md                             # this file
└── pdf-examples/                         # sample inputs/outputs used to verify split & merge logic
    ├── quarterly-report.pdf              # source, 9 pages
    ├── cover-page.pdf                    # source, 1 page
    ├── appendix.pdf                      # source, 3 pages
    ├── quarterly-report_part1_pages1-3.pdf
    ├── quarterly-report_part2_pages4-6.pdf
    ├── quarterly-report_part3_pages7-9.pdf
    └── board-briefing-merged.pdf
```

## Processed PDF examples

To validate the split/merge logic independent of the UI, sample documents were generated and run through the same operations the app performs.

**Source documents**

| File | Pages | Size |
|---|---|---|
| `quarterly-report.pdf` | 9 | 6.0 KB |
| `cover-page.pdf` | 1 | 1.6 KB |
| `appendix.pdf` | 3 | 2.8 KB |

**Split — Custom ranges mode:** `quarterly-report.pdf` split into ranges **1–3**, **4–6**, **7–9**

| Output | Pages | Size |
|---|---|---|
| `quarterly-report_part1_pages1-3.pdf` | 3 | 2.3 KB |
| `quarterly-report_part2_pages4-6.pdf` | 3 | 2.2 KB |
| `quarterly-report_part3_pages7-9.pdf` | 3 | 2.2 KB |

**Merge:** `cover-page.pdf` + `quarterly-report_part1_pages1-3.pdf` + `appendix.pdf`

| | |
|---|---|
| Inputs | 1 + 3 + 3 = 7 pages |
| Output | `board-briefing-merged.pdf` — **7 pages, 5.3 KB** |

Both operations produced the exact expected page counts and page order, with no dropped or duplicated pages.

## Key learnings

- **Splitting reduces to one internal model.** Every mode (ranges, breakpoints, every-N, extract) is really just computing a list of `{filename, pages[]}` groups before any file is touched. Modeling it that way let four different UI interactions share a single execution path.
- **Validate on every input, not on submit.** Moving validation to fire live — and disabling the primary action until the plan is fully valid — made the tool feel trustworthy instead of punishing.
- **Lazy-render thumbnails or the tab freezes.** Eagerly rendering every page canvas on a 200+ page PDF is visibly janky. Using `IntersectionObserver` to render only what's scrolled into view (with a skeleton placeholder) keeps the UI responsive at any document size.
- **`pdf-lib` and `pdf.js` aren't interchangeable.** `pdf-lib` builds and copies pages but can't render anything visually; `pdf.js` renders but can't reassemble documents. Tools that skip one end up either blind (no previews) or unable to produce output — both were needed, each fed its own copy of the file's array buffer.
- **One signature interaction beats decorating everything.** Narrowing the app's "personality" down to two moments — a scissors icon that snips on a split, a paperclip connecting files in the merge list — kept the UI memorable without turning into visual noise.
- **No backend is a feature, not a limitation.** Skipping a server entirely removes upload wait time, arbitrary file-size caps, and any question of where a sensitive document went — which turned out to be the tool's strongest differentiator.

## Accessibility

- Full keyboard operability (tabbing, arrow-key reordering, `Enter`/`Space` activation)
- Visible focus states throughout
- ARIA roles/labels on tabs, checkboxes, and live regions for status/error announcements
- `prefers-reduced-motion` respected — animations are disabled when requested
- Sufficient color contrast in both light and dark themes

## Roadmap ideas

- [ ] Optional page rotation before export
- [ ] Drag-to-reorder pages within the split thumbnail grid
- [ ] PDF compression pass on output
- [ ] Batch merge presets (save a file order as a template)

## License

MIT — do whatever you'd like with it.
