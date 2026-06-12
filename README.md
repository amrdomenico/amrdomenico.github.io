# Personal Website

My personal website and portfolio, live at [amrdomenico.github.io](https://amrdomenico.github.io).

## Stack

No framework, no build step, no external dependencies. Plain HTML, CSS, and JS, enough for the scope of the project. Any file opens directly in the browser and deploying is a `git push`.

## File structure

```
assets/                    Shared CSS (base.css, project-shared.css) + per-page CSS
js/                        main.js — the only shared JS file
projects/
  venomannotationpipeline/ index.html — Venom Annotation Pipeline
  spoilerguard/            index.html — Spoiler Guard
  agentictodo/             index.html — Agentic Todo List
index.html                 Home + resume (SPA with hash routing)
```

Each page loads only what it uses. `spoiler.html`, for example, loads `base.css + project-shared.css + spoiler.css`. The same applies to JS: `main.js` is the only shared file. Page-specific logic lives in an inline `<script>` in the HTML itself.

## index.html as a SPA

`index.html` holds two sections (`#home` and `#resume`) that toggle via hash routing, with no page load. The motivation is responsiveness: home and resume are the first-impression pages, where someone visiting the site is likely scanning the profile quickly. Avoiding a full navigation between them makes that experience smoother.

The mechanism is straightforward: `hashchange` shows/hides the `.page-section` divs and updates the active nav state.

```js
window.addEventListener('hashchange', handleHash);
handleHash(); // runs on initial load
```

The resume is also printable directly from the page. `@media print` removes the nav and the card wrapper, leaving only the content.

Project pages (`projects/`) don't follow this logic. Anyone clicking into a project is already interested in reading, so a normal navigation is acceptable. Each project lives in its own subdirectory (`projects/venomannotationpipeline/`, etc.) with an `index.html`, which keeps URLs clean and avoids exposing file extensions.

## i18n

Two languages (PT/EN) with the preference persisted in `localStorage`. The engine lives in `main.js` and works in two layers: `_sharedI18n` with keys common to all pages (nav, labels), and `initI18n({ pt: {}, en: {} })` with page-specific keys, called inline in each HTML file. Both are merged on initialization. `applyLang(lang)` iterates over all `[data-i18n]` elements and replaces their content. Strings that contain HTML (such as inline `<code>`) use `innerHTML` intentionally. The default language is `'en'`. The fallback for invalid values is also `'en'`.

## CSS

`base.css` defines the design tokens (colors, shadows, radii, typography) and global styles (nav, layout, footer). Pages don't override tokens, they only extend them.

The color hierarchy uses semantic naming:

```css
--ink           /* primary text */
--ink-mid       /* secondary text */
--ink-light     /* supporting text */
--ink-faint     /* placeholder, decorative */
--accent        /* terracotta red — actions, highlights */
--olive         /* olive green — tech stack, indicators */
```

`--white` is a warm off-white (`#fffef9`), not pure white. An intentional choice to pair with the `--bg` background.

## Deploy

Hosted on GitHub Pages. Any push to `main` is live immediately. No CI, no build step.
