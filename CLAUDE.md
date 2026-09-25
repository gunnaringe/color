# color

Full-screen random color picker for kids' games, served at color.haxxor.xyz.

- **Single file:** everything (markup, CSS, JS) lives in `public/index.html`. No
  build step, no dependencies, no framework. Keep it that way.
- **Deploy:** Cloudflare Worker with static assets. `wrangler.jsonc` pins the
  Worker name `color-haxxor-xyz` and the `color.haxxor.xyz` custom domain — never
  rename the Worker, the domain is bound to it. Only `public/` is uploaded.
  A git push does **not** deploy unless Workers Builds is connected; otherwise the
  user runs `npx wrangler deploy`. Agent sessions can't deploy (Cloudflare
  credentials there are read-only) — say plainly that the live site is unchanged.
- **i18n:** all UI strings go in the `I18N` object (`no` and `en`) and are wired
  via `data-i18n` / `data-i18n-html` / `data-i18n-title`. Built-in colors
  (`BUILTIN`) carry a `key` whose translated name is the default; a custom `name`
  overrides it, clearing it restores the default. Recoloring drops the `key`.
- **Settings** persist in `localStorage` under `color-settings-v1`. `load()`
  merges stored settings over `defaults()`, so adding a new setting just needs a
  default; bump the key only for incompatible changes.
- **Share links:** every `save()` also mirrors settings into the URL fragment
  (`encode`/`decode`, format documented above them), omitting defaults. A
  fragment on load wins over `localStorage`. Links are a public contract —
  only add new keys, never change the meaning of existing ones.
- **Style:** playful but tidy — rounded font (Fredoka), circles and pills only,
  no square boxes or sharp corners. Springy `--ease-pop` transitions.
- **Testing:** open the file in headless Chromium via Playwright; check space,
  tap, the pause modes and that settings survive a reload.
