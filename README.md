# Personal CV

The static CV site served at **[phillongworth.co.uk](https://phillongworth.co.uk)** — a single-page
CV plus two project write-ups. No framework, no build step: plain HTML and CSS, deployed as files.

## Structure

| Path | Purpose |
|---|---|
| `index.html` | The CV itself — profile, experience, skills, and a "Major projects" list linking to the two project pages |
| `project1.html` | Kirklees Health and Wellbeing Strategy |
| `project-2.html` | Kirklees Healthy Working Life Programme |
| `style.css` | All styling, including a `[data-theme="dark"]` block |
| `assets/resume.pdf` | Downloadable CV, linked from the header |
| `assets/images/` | Profile photography |
| `js/script.js` | Would stamp a "last updated" date — see *Known quirks* |

## Working on it

There is nothing to install. Open `index.html` directly, or serve the folder if you want relative
paths to behave exactly as they do in production:

```bash
python3 -m http.server 8000
```

## Deploying

The site is served by nginx from the checkout itself, behind Cloudflare, so committed changes to the
working tree on the server are already live — there is no build or copy step. Push to `main` for the
canonical copy; nothing redeploys automatically.

`deploy.ps1` is **not** a working deploy path. It scp's to a `$SERVER` placeholder still set to
`"myserver"`, and targets an `html/` subdirectory that does not exist under the web root. It is kept
only as a starting point if a push-based deploy is ever wanted; don't run it expecting a result.

## Known quirks

These are all deliberate or harmless, and are recorded here so they don't get "fixed" twice:

- **`js/script.js` is never loaded.** No page includes a `<script>` tag for it. It looks for a
  `#last-updated` element and sets it from `document.lastModified`; neither the script nor the
  element is wired up.
- **`assets/images/phil_longworth_headshot_2026.png` is unused.** `index.html` still points at
  `phillongworth.jpg`. The newer headshot is committed but not referenced anywhere.
- **The dark theme never activates.** `style.css` defines a full `[data-theme="dark"]` palette, but
  no page sets `data-theme` and there is no toggle, so the light palette always applies. If you wire
  up a toggle, note the light variables live on bare `:root` and the dark ones only inside the
  attribute selector — a mismatch between those two blocks previously left light mode with undefined
  colour variables, which rendered links as plain black text.
