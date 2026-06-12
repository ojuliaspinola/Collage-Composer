# Nightly Composer

A single-file photo composition web app — a low-friction nightly creative ritual.
Pick a background, choose a layout, drop in a couple of photos, write a line or two,
generate a 1080×1080 image, save it. No accounts, no persistence, no build step.

**Live site:** https://ojuliaspinola.github.io/Nightly-Composer/

## Local preview

Open `index.html` directly in a browser, or run any static server:

```bash
python3 -m http.server 8000
# → http://localhost:8000
```

## The five formats

1. **Two photos + text** — two photos side by side, text underneath
2. **Three photos + text** — three photos in a row, text underneath
3. **One photo + wrapping text** — centred photo, text wraps around it
4. **Two overlapping photos + text** — double-exposure, text framing the edges
5. **Randomizer** — two photos, two text lines, re-roll to shuffle position & size

## Structure

- `index.html` — the entire app (vanilla HTML/CSS/JS, Canvas rendering, inline PWA manifest)
- `ComposerV1/` — archival snapshot of v1
  - `nightly-composer.html` — original app file (same content as `index.html`)
  - `nightly-composer-code.txt` — plain-text copy of the code, for reading anywhere
  - `nightly-composer-design.md` — design document (the *why*: purpose, principles, key decisions)

The design doc is the important one to read first if you're returning to this months
later — it captures the non-obvious decisions (especially **no persistence** and
**constraint creates voice**) that are easy to lose track of and tempting to override.

## Deploy (GitHub Pages)

This repo serves the root `index.html` via GitHub Pages:

1. Push to GitHub.
2. Repo → **Settings → Pages**.
3. Source: **Deploy from a branch** → `main` / root.
4. Site goes live at `https://<user>.github.io/<repo>/`.

To update the live app, edit `index.html` and push to `main`.
