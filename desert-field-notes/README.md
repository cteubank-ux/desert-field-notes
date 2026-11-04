# Desert Field Notes — Repair Submission

## Summary
This repository contains the repaired Desert Field Notes readiness-audit project.

## Repair log

| Area | Condition / evidence | Root cause | Correction |
|---|---|---|---|
| Resource-page paths | `pages/resources.html` referenced `/css/styles.css` and `/index.html`. Those root-relative paths fail when the site is hosted below a domain root, such as a GitHub Pages project site. | Paths were written relative to the domain root rather than the nested `pages/` directory. | Changed the stylesheet path to `../css/styles.css` and home links to `../index.html`. Kept `resources.html` local to the current directory. |
| Viewport | The home page declared `width=980`, forcing a 980 CSS-pixel layout viewport on narrow screens. | A fixed viewport width was used instead of the device width. | Changed it to `width=device-width, initial-scale=1`. |
| Image alternative | The image used `alt="cactus"`, which did not convey the meaningful visual content. | The alternative was too generic to communicate the illustration. | Described the green saguaro, desert horizon, and warm sky in the `alt` text. |
| Responsive grid | `grid-template-columns:(auto-fit, minmax(325px, 1fr));` is invalid CSS syntax. | The `repeat()` function was omitted. | Used `repeat(auto-fit, minmax(min(100%, 325px), 1fr))` so columns can fit narrow containers without a 325px minimum forcing overflow. |
| C4 — featured-card cascade | The same element had `id="featured-card"` and `class="featured"`. The ID selector `#featured-card` had higher specificity than `.featured`, so the two background declarations competed and the ID rule won. | Conflicting selectors with unequal specificity targeted the same element. | Replaced both rules with one `.card.featured` rule and removed the obsolete ID-specific background rule. |

## Testing evidence

The project was checked with local automated source checks. These checks verify markup references and CSS parsing; they are not a substitute for the required interactive browser and keyboard tests.

| Test | Expected result | Actual result |
|---|---|---|
| Local file-reference check on both HTML pages | Every local stylesheet, image, and navigation destination resolves to an existing project file. | PASS — all local references resolve after correcting the home-page Resources link and nested-page paths. |
| CSS parser check | Stylesheet parses without CSS syntax errors. | PASS — parser reported zero stylesheet-level syntax errors. |
| Viewport declaration check | Home page uses the device-width viewport rather than a fixed 980px viewport. | PASS — source declares `width=device-width, initial-scale=1`. |
| Image alternative check | Informative SVG has a descriptive alternative. | PASS — non-empty alternative describes the saguaro, horizon, and sky. |
| Grid declaration check | Grid uses valid `repeat()` syntax and a narrow-container-safe minimum. | PASS — source declares `repeat(auto-fit, minmax(min(100%, 325px), 1fr))`. |
| C4 cascade source check | No ID-specific background rule competes with the featured class rule. | PASS — one `.card.featured` background rule remains. |

**Browser evidence still required before submission:** open the site at narrow (375px), medium (600px), and desktop (1280px) widths; confirm no horizontal page scrolling; inspect the featured-card color; and use Tab to verify visible focus on links. Record the actual browser, viewport sizes, and observations here. Do not report these browser checks as completed until you perform them.

## Code defense

- **Relative paths:** `../` moves from the `pages/` folder to the project root. This makes the resources page work on both a local server and a project-site path such as `https://username.github.io/repository/`.
- **Viewport:** `device-width` makes the layout viewport match the device's CSS-pixel width. It does not by itself make fixed-width content responsive; the CSS must also fit the available space.
- **Image alternative:** Alternative text should communicate the informative content of the image, not merely name its broad category.
- **Grid:** `repeat()` is required to repeat the auto-fit track pattern. `min(100%, 325px)` prevents the minimum track size from exceeding a very narrow available container.
- **C4 cascade:** Removing the competing ID selector gives the featured-card background one clear class-based source of truth. This avoids relying on an ID's higher specificity to override a class.

## Repository and publishing steps

**Repository URL:** https://github.com/cteubank-ux/desert-field-notes.git

**Published URL:** 

