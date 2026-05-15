# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository purpose

This repo hosts the two German-law-mandated legal pages for the **Quitto** iOS app (a bookkeeping app for §19-Kleinunternehmer / German small-business owners). The pages are served as static HTML, linked from the App Store listing and from within the app.

- `impressum.html` — Legal notice per §5 TMG (operator details, contact, liability disclaimers).
- `datenschutz.html` — GDPR privacy policy. Describes how the app processes data: local-only storage, plus named processors for the Premium OCR feature (Cloudflare Worker + Claude API), subscriptions (RevenueCat), payments (Apple IAP), and crash reporting (Sentry).

There is no build system, no test suite, no package manager, and no shared assets. Each HTML file is fully self-contained with inline `<style>` blocks. Preview locally by opening the file directly in a browser, or `python3 -m http.server` from the repo root.

## Working with these files

- **Content is legally binding in Germany.** Treat substantive text changes (operator address, listed processors, retention periods, legal bases under GDPR Art. 6, §-references) as factual edits that the user must confirm — do not paraphrase or "tidy up" legal wording on your own. Styling, layout, and typo fixes are fine without confirmation.
- **Adding a new data processor to the privacy policy** requires a new numbered `<h2><span class="section-num">N</span>...</h2>` section in `datenschutz.html` following the existing pattern: what data is sent, legal basis (typically Art. 6 Abs. 1 lit. b or f DSGVO), and processor location. Renumber subsequent sections.
- **Bump the "Stand: <Month Year>" line** at the top of `datenschutz.html` (around line 335) and the footer of `impressum.html` whenever substantive content changes. The user's current date is the source of truth.
- **Keep the two files visually independent.** They intentionally use different design languages (impressum.html: Apple-style system fonts with orange `#ff9500` accent; datenschutz.html: Inter font with navy/teal gradient and a sticky top bar). Don't try to unify them unless asked.
- **All user-facing text is German.** Do not translate or add English copy. Section headings, legal references (TMG, UStG, DSGVO/GDPR, RStV), and the formal "Sie"-form must be preserved.

## Git workflow

Branch convention used by this session: `claude/<topic>-<suffix>` (e.g. `claude/add-claude-documentation-Ts1Mm`). After pushing, open a draft PR against `main`. Commit messages in history are short, German or English, single-line, describing the legal change (e.g. `Sentry als Auftragsverarbeiter ergaenzt (DSGVO Art. 13)`).
