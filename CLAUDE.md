# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

Tristan Endo's personal site, a GitHub Pages user site served from the root of the default branch at
`https://tristane028.github.io/`. It has no build step, package manager, linter or tests. Pushing to the default
branch deploys it.

Preview locally:

```bash
python3 -m http.server 8000
```

Then open `http://localhost:8000/`. Check changes at phone width as well as desktop.

## How to work here

The full rules are in the `tristan-projects` skill. These are the parts that always apply in this repo:

- **Think first.** State assumptions, lay out readings of an unclear request instead of picking one, and ask when unsure.
- **Keep it small.** Write the least code that solves the problem, and touch only what the request needs. Match the existing style, and mention unrelated problems instead of fixing them.
- **Three designs first.** For any new section, layout change or visual, make three different designs. Compare them on readability, fit with the site, accuracy of the data and effort, recommend one, and wait for Tristan to pick before building. Bug fixes and small copy edits skip this step.
- **Verify before reporting.** Serve the page, screenshot it headless at desktop and phone width, check the console for errors, and confirm Tristan's own text is unchanged.
- **Git.** Pushing to `main` publishes the site, so work on a branch, commit and push it, and let Tristan merge.
- **Copy.** Write casual but professional, first person and specific, in his voice from the `writeup` skill. No contractions, no em dashes and no semicolons. Some existing copy has contractions ("I'm", "haven't"). Leave it alone unless asked, but do not add new ones.

## Structure

- `index.html` is the whole site. It's a single page with HTML, CSS and JS all inline and no external scripts.
  The only external dependency is Google Fonts.
- `profile.jpg` is the headshot used in the hero.
- `Endo_Curriculum_Vitae_MM_DD_YYYY.pdf` are dated copies of the CV. Older ones stay in the repo because links
  elsewhere may point to them. When adding a new CV, update **both** CV links in `index.html` (header nav and footer
  nav) to the new filename.
- `google017456d0cc86377d.html` is Google Search Console verification. Don't rename or delete it.

## Sibling projects linked from the page

Several cards link to other repos that GitHub Pages serves as project sites under the same domain:
`/Springer-Fibers/Visuals.html`, `/Starlight-Pokemon-Datasheet/` (Project Astral) and `/Aerodynamic-Model/`
(Kestrel Kinematics). Changes to those pages belong in their own repos, not here.

## Conventions in `index.html`

- **Styling is almost entirely inline `style=""` attributes**, with only a tiny `<style>` block for `body` and link
  colors. Match this when adding sections. Don't introduce a stylesheet or framework unless asked.
- Layout is flexbox/grid with `flex-wrap` and `min-width: 0` for responsiveness. There are no media queries.
  Sections share `max-width: 1200px; margin: 0 auto; padding: … 40px`.
- Palette: background `#0e0e10` (cards `#121214`, figure insets `#0b0b0c`), text `#e8e4da`, muted text
  `#b9b4a9` / `#a39e93`, gold accent `#c8a25a` (hover `#e2c07e`, highlight `#f3d48f`), borders `#2a2823` /
  `#3a3629`.
- Fonts: Cormorant Garamond for headings, Inter for body text, IBM Plex Mono for eyebrow labels and captions, and
  Noto Serif JP for the 遠藤 monogram.
- Sections are anchored by id (`#top`, `#findings`, `#notebook`, `#outside`, `#contact`), and the header nav links
  to them. Keep the nav in sync if sections are added or renamed.
- Findings cards follow a fixed pattern: a numbered mono eyebrow (`01 · Topic · status`), an `h3` claim stated as
  the result, body text, collaborators/tools line, links, and a `figure` with gold corner brackets holding an SVG
  sketch.
- Copy is plain, first-person and specific. Headlines state the finding rather than the topic.

## Hero animation (the only JS)

The `<script>` at the bottom draws exact fractional Brownian motion samples (H = 0.7) into the hero SVG. It
Cholesky-factors the fBm covariance once, then samples paths as `L·z`. The shaded band is ±2·t^H, and segments
outside it (after `HERO_T_MIN`) are redrawn in the highlight color with a dot at the worst escape. Six fixed
`<path>` slots (`hero-p0..5`, `hero-h0..5`, `hero-m0..5`) are rotated every 3.5 s, oldest to newest by opacity.
The animation is skipped under `prefers-reduced-motion`. The script uses ES5 style (`var`, function expressions),
so keep to that.

## Known issue

The ReLU "Correlation vs depth" chart in Findings 02 has `points="undefined"` on its three `<polyline>`s, so the
lines don't render. It looks like the markup was exported from a template whose data never got filled in. The
points need to be computed (iterate the ReLU arc-cosine correlation map from ρ₀ = 0, 0.3, 0.6 over 12 layers) and
written in, or generated by script.
