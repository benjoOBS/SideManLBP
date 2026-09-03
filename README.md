# Sideman

A browser-based mix analyzer and live-sound assistant — listens through a mic
or a loaded recording and helps spot frequency conflicts, harshness, and
tonal balance issues in real time.

**[Live demo](https://benjoOBS.github.io/REPO-NAME/)**
*(update this link with your actual repo name once Pages is live)*

## What it does
- Real-time spectrum analyzer (microphone, recorded file, or system audio)
- Per-instrument capture and tagging (category, lead/support, mic type)
- Frequency-conflict, harshness, and thinness/boxiness detection
- Venue/room profiling with rough room-resonance math
- Target curves — built-in presets, or capture your own from a reference track
- Named saved setups, export/import, and a live audition EQ + compressor preview

## Setup
No build step — it's a single self-contained HTML file. Open it directly, or
host it via GitHub Pages (Settings → Pages → deploy from the `main` branch).

**Mobile note:** microphone and camera access require HTTPS (or localhost) —
GitHub Pages covers that automatically. Opening the file directly via
`file://` will block both on most browsers.

## Data & privacy
Everything is stored locally in your browser (`localStorage`) — nothing is
uploaded anywhere. Clearing your browser's site data clears everything
Sideman has saved.

## Manual
See `sideman-user-manual.md` for a full feature-by-feature guide, including
the known limitations of each.

---
Built by Benji ([@benjoOBS](https://github.com/benjoOBS))
