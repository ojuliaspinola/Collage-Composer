# NIGHTLY COMPOSER — DESIGN DOCUMENT

## Purpose

Nightly Composer is a single-file photo composition web app, built as a structured nightly creative ritual. The aim is to replace passive phone-scrolling with a small, low-friction creative act each evening: pick a background, choose a layout, drop in a couple of photos, write a line or two, generate a square image, save it.

The brief came from a frustration with existing photo editing apps — none did exactly the thing wanted, which was: take a small number of inputs (photos + text), arrange them according to a predetermined aesthetic, and produce something coherent in seconds. The whole point is that the *constraint* is the practice. By predetermining the layouts and typography, every output becomes part of a series with a consistent visual language, rather than another decision-by-decision edit.

## Design principles

**Low friction over features.** The activation energy for a creative habit has to be near zero. The flow is deliberately five steps with one decision per step. No accounts, no saving state, no cloud, no settings panel. Open the app, make a thing, save it, close. The whole loop should take under two minutes.

**Coherent series, not one-offs.** Fixed typography (VT323) and a small set of layouts mean outputs from different nights will feel like they belong together. Over weeks or months they accumulate into a body of work without anyone having had to think about visual identity.

**Constraint creates voice.** No infinite customisation. The user picks among presets, not parameters. This is the opposite of Photoshop — choices are the practice, not the obstacle.

**Speak-now, refine-later.** Like the Kemmer worldbuilding model, the goal is to get the user *making* with minimum friction in the moment. Polishing or curating happens later, separately, away from the act of creation itself.

**Phone-first.** Everything is designed for one-thumb operation on a vertical phone screen. The app is a webpage so it sidesteps the entire mobile app build pipeline; "Add to Home Screen" makes it indistinguishable from a native app in daily use.

**Aesthetic signature.** The UI itself uses VT323 (the same font used in the output compositions) and a dark monochrome palette — early-computer / terminal vibes. This connects the tool aesthetically to the user's broader creative work in video games and worldbuilding, where retro-tech visual language is already a recurring motif.

## Architecture

The entire application is a single self-contained HTML file. No build step, no framework, no dependencies installed locally. Vanilla HTML/CSS/JavaScript. The only external resources are the VT323 font (loaded from Google Fonts via CDN) and nothing else.

This choice is deliberate. A single file:
- Can be edited from any device with a text editor
- Can be deployed with a literal drag-and-drop to Netlify
- Has zero maintenance burden — no `npm install`, no security updates, no deprecated dependencies
- Will still work in five years

The rendering pipeline uses HTML Canvas. The user's choices are collected into a state object, then on Generate the canvas is sized to 1080×1080 and a layout-specific drawing function paints background → photos → text into the canvas. The final canvas is converted to a PNG blob and presented to the user as an inline image they can long-press to save (the most reliable Android save pattern).

The app is also installable as a Progressive Web App (PWA) via an inline web manifest, so it appears in the user's home screen alongside native apps and launches in its own window without browser chrome.

## Flow

1. **Choose background** — Upload an image (cover-fit to canvas) or use plain white
2. **Choose format** — One of five presets
3. **Upload photos** — Quantity depends on chosen format
4. **Write text** — One textarea (two for randomizer)
5. **Generate** — Canvas renders; user saves the PNG

## The five formats

1. **Two photos + text** — Two photos side by side sitting slightly high in the canvas, text underneath. The original spec — meme-poster / zine style.

2. **Three photos + text** — Three photos in a horizontal row, text underneath. For when one photo isn't enough but two photos are too symmetric.

3. **One photo + wrapping text** — A square photo centred, with text genuinely wrapping around it in left and right columns and continuing full-width above and below. Magazine layout, for when text is the primary content.

4. **Two overlapping photos + text** — Both photos rendered uncropped (contain-fit), centred at 50% opacity, producing a double-exposure effect. Text tiles around all four edges of the canvas as a frame, separated by bullet points. The most experimental of the structured layouts.

5. **Randomizer** — Two photos and two optional text lines, with a Re-roll button on the result screen. Each re-roll randomises position and size of every element (no rotation — kept upright). This is the play mode: the others are for making a specific thing; this one is for stumbling onto something.

## Design decisions worth recording

**VT323 over alternatives.** Chosen for the early-computer / monochrome-monitor aesthetic. Reads as terminal/cyberpunk without being aggressive about it. Press Start 2P (8-bit arcade) was considered but felt too retro-game; VT323 sits in early-PC territory, which suits the user's broader visual vocabulary better.

**1080×1080 square output.** Square so outputs work as memes, Instagram posts, or just photos. 1080 matches Instagram's recommended resolution, future-proofs for high-DPI displays.

**Long-press to save, not download API.** Mobile browsers (especially Android Chrome) are inconsistent about programmatic downloads. The "show the image fullscreen, user long-presses → Save image" pattern uses the browser's native save mechanism and works reliably across Android.

**Inline PWA manifest.** Avoiding a separate `manifest.json` file keeps everything in one file. The manifest is embedded as a data URI on a `<link rel="manifest">` tag, with the icon also inline as an SVG data URI.

**No persistence.** Deliberate. The app doesn't remember past sessions, doesn't save drafts, doesn't have an account. Each composition is a discrete act. Persistence would invite curation-in-the-moment, which is exactly the friction the project is designed to avoid.

**No image filters or colour treatments on photos.** Considered (grain, desaturation, paper borders) and rejected. Photos are inputs to the composition, not the subject of editing. The aesthetic is in the typography, layout, and background — not in retouching the user's photos.

## Hosting

The app is hosted on Netlify (free tier). Updates: drag the updated HTML file onto the Netlify dashboard, redeploy, done.

## Possible future additions

Things considered but not built — to keep in mind if the tool gets used long enough that adding them feels worth it:

- More format presets (vertical strip, asymmetric grid, full-bleed single photo with corner text)
- Subtle text legibility help for layouts where text sits on busy backgrounds (semi-transparent panel, soft shadow)
- A way to set a "default background" so users with a recurring aesthetic don't re-upload the same image every night
- Multi-line input for randomizer (each line = separately-placed fragment)
- A "Surprise me" mode that randomly picks one of all the formats including the structured ones
- Output dimensions beyond square (portrait 4:5 for Instagram, story 9:16)
- A lightweight gallery view showing the past N outputs (would require giving up the no-persistence principle — only worth it if the lack of memory becomes the actual blocker)

## File inventory

- `nightly-composer.html` — the entire app
- `nightly-composer-code.txt` — a `.txt` copy of the same file, for archival / reading purposes
- `nightly-composer-design.md` — this document
