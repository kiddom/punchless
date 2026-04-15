# Hole Punch Remover

Removes printed hole-punch marks from scanned/digital PDFs. Detects the gray circles and vertical border lines added by print workflows and covers them with white rectangles, producing a clean PDF.

## Web App

A single-page client-side web app — no server, no uploads, all processing happens in the browser.

**[Open the web app](https://kiddom.github.io/blankout/)**

### How it works

1. Drop one or more PDFs onto the page
2. The app scans every page for hole-punch circles (gray stroked paths matching a specific color and 22.5pt diameter)
3. White rectangles are drawn over detected marks via a PDF incremental save
4. Download the cleaned PDFs as a zip

### Technical details

- **Detection** — pdf.js operator-list parsing with CTM tracking. No canvas rendering. ~40ms/page.
- **Modification** — Raw PDF byte manipulation with incremental save. Original bytes are never modified; new objects and a cross-reference stream are appended.
- **Large file support** — Handles 1,000+ page PDFs. No file size limits beyond browser memory.
- **Dependencies** — [pdf.js 3.11](https://mozilla.github.io/pdf.js/) (detection), [JSZip 3.10](https://stuk.github.io/jszip/) (multi-file download). Both loaded from CDN.

## Detection parameters

Both versions use the same detection criteria:

| Parameter | Value | Description |
|-----------|-------|-------------|
| Hole color (RGB) | (188, 190, 192) | Target gray, 8-bit |
| Color tolerance | 10 | Per-channel tolerance |
| Circle diameter | 22.5pt | Expected hole-punch size |
| Size tolerance | 5pt | Accepted deviation |
| Circle buffer | 5pt | White rect padding around each circle |
| Expected circles | 3 | Per page (top, middle, bottom) |
