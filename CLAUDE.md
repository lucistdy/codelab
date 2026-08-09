# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

"Codelab" is a small collection of standalone, illustrated, interactive HTML explainers for CS/math topics (currently modular arithmetic / primality testing, and fast exponentiation). There is no build system, package manager, or test suite — every page is a single self-contained `.html` file with inline `<style>` and inline `<script>`, meant to be opened directly in a browser or served as static files.

## Commands

There is no build, lint, or test tooling in this repo. To preview a page, just open it directly:

```bash
open index.html                 # macOS
# or serve locally, e.g.
python3 -m http.server
```

Any manual verification is done by opening the file in a browser and exercising the interactive widgets (input fields, "run" buttons, `<details>` quizzes).

## Site structure

- `index.html` / `index_cn.html` — landing pages (English / Chinese) that link out to the explainer pages via `.card` links.
- `prime_detective_en.html` / `prime_detective_cn.html` — the main content piece: an 8-"case" narrative ("The Prime Number Detective Agency") walking through remainders, modular addition, fast exponentiation, primality testing, the Sieve of Eratosthenes, and the Fermat / Miller–Rabin primality tests. Sections are `<section class="case" id="caseN">`.
- `fast_exponentiation_en.html` / `fast_exponentiation_zh.html` — a deeper-dive "skill refresher" side page that expands on Case 03 of the main case file, structured as `<section class="level" id="levelN">` steps 1–8.

**Note the inconsistent locale suffix**: index and prime_detective pages use `_cn`, but fast_exponentiation uses `_zh`. Preserve whichever suffix an existing file already uses; don't rename.

## Bilingual pairing convention

Every content page exists as an English/Chinese pair that must stay structurally identical: same section IDs, same DOM structure for interactive widgets, same inline JS logic — only prose and labels are translated. When editing one language's content or behavior, mirror the change in its counterpart. Each page's `.lang-switch` links directly to its sibling in the other language (not just back to the index).

## Page anatomy (shared conventions across all content pages)

Each explainer page follows the same template, so understanding one makes the others easy to edit:

- **Theming**: CSS custom properties (`--paper`, `--ink`, `--accent`, `--code-bg`, etc.) are defined once on `:root`, then overridden for dark mode two ways: a `@media (prefers-color-scheme: dark)` block (the only one actually active) and `:root[data-theme="dark"]` / `:root[data-theme="light"]` blocks that are currently unused hooks (no JS sets `data-theme` today) — keep both in sync if you touch the palette, in case a manual toggle is added later.
- **Layout building blocks** (reused by class name across pages): `.hero` (title banner), `.level`/`.case` sections with a numbered badge, `.callout` boxes (`.fact`/`.tip`/`.warn` variants), `.codebox` for syntax-highlighted code (spans use `.kw`/`.cm`/`.st`/`.nu` for keyword/comment/string/number), `.annot`/`.annot-row` for code-with-side-notes walkthroughs, `.chain`/`.bits` for visualizing doubling/binary, `.stats` stat tiles, `table.trace` for step-by-step algorithm traces, `.widget` for the interactive tool, and `<details>` for spoiler-gated quiz questions.
- **Interactive widgets**: implemented in a plain `<script>` block at the end of the page (IIFE, no dependencies). The widget reads `<input>` values, re-implements the algorithm in JS to mirror the Python shown in the `.codebox`, and renders a step-by-step `table.trace` into a `#out` div. When adding a new interactive demo, follow this same pattern rather than pulling in a framework.
- Pages close with a `.footer` that cross-references other cases/pages in the series (e.g. fast_exponentiation footer points back to the main case file; the main case file's Case 03 links out to fast_exponentiation as an optional deep dive).

## Editing guidance

- Keep pages dependency-free (no external CDN scripts/fonts/stylesheets) — everything is inline so a page works when opened as a raw local file.
- When adding a new explainer, create both language variants, link them from `index.html`/`index_cn.html`, and add a `.lang-switch` link between the two new pages.
