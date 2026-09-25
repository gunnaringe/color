# color

Full-screen random color picker for kids' games, served at color.apphub.casa.

- **No build:** the app is `public/index.html` (markup, CSS, JS inline) plus the
  PWA files next to it: `manifest.webmanifest`, `sw.js`, `icons/` and
  self-hosted Fredoka in `fonts/` (SIL OFL — keep `fonts/OFL.txt`). No build
  step, no dependencies, no framework, no third-party requests (so it works
  offline). Keep it that way.
- **Service worker:** the page is network-first (deploys show up on the next
  load), other files cache-first with background refresh. Bump `CACHE` in
  `sw.js` only when changing the precache list or strategy. Service workers
  don't run from `file://` — test PWA/offline over `python3 -m http.server`
  in `public/`.
- **Deploy:** Cloudflare Worker with static assets. `wrangler.jsonc` pins the
  Worker name `color` and the `color.apphub.casa` custom domain — never
  rename the Worker, the domain is bound to it. Only `public/` is uploaded.
  Workers Builds is connected: a push to `main` deploys (if a push doesn't,
  the Cloudflare GitHub app probably lacks access to this repo). Agent sessions
  can't deploy by hand (Cloudflare credentials there are read-only).
- **i18n:** all UI strings go in the `I18N` object (`no` and `en`) and are wired
  via `data-i18n` / `data-i18n-html` / `data-i18n-title`. Built-in colors
  (`BUILTIN`) carry a `key` whose translated name is the default; a custom `name`
  overrides it, clearing it restores the default. The color picker is custom
  (no native `<input type=color>` — its popup is unstyled and half-translated):
  picking one of the `PRESETS` sets `key` to that preset, any other hex drops it,
  and `decode` maps preset hexes in links back to their `key`.
- **Settings** persist in `localStorage` under `color-settings-v1`. `load()`
  merges stored settings over `defaults()`, so adding a new setting just needs a
  default; bump the key only for incompatible changes.
- **Share links:** `localStorage` is the single source of truth. The Share
  button encodes settings into a URL fragment (`encode`/`decode`, format
  documented above them, defaults omitted). Opening a link applies it once
  (`takeHash` saves it and strips the fragment) and shows an undo toast. Links
  are a public contract — only add new keys, never change existing ones.
  User-provided text (setup/color names) comes from links: always render it
  with `textContent`, never `innerHTML`.
- **Style:** playful but tidy — rounded font (Fredoka), circles and pills only,
  no square boxes or sharp corners. Springy `--ease-pop` transitions.
- **Testing:** open the file in headless Chromium via Playwright; check space,
  tap, the pause modes and that settings survive a reload.
