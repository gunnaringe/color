# Farger · Colors

A full-screen random color picker for kids' games: each color means an activity
("yellow = jump ten times"). Press **space** (or tap the screen) to reveal a random
color.

Live at **[color.haxxor.xyz](https://color.haxxor.xyz)**.

## Features

- Starts on a black screen; space / Enter / tap shows a random color.
- Customizable colors: tap the circle to pick a color, give it a name (optional).
  Defaults are yellow, green and blue.
- Optional pause screen between colors — either on the next tap or automatically
  after a number of seconds — in a color of your choice (black by default).
- "Never the same color twice in a row" and "show the color's name" toggles.
- Norwegian and English (auto-detected, or pick one in settings).
- Fullscreen button, and keeps the screen awake while playing.
- Settings are stored in the browser (`localStorage`) and mirrored into the URL,
  so the address bar is always a share link — **Share setup** copies it (or opens
  the share sheet on phones). Example:
  `color.haxxor.xyz/#c=y:Hopp,g:Sitt,e63946:Snurr&p=5` (format documented in
  `public/index.html` above `encode`).

## Development

Everything is in [`public/index.html`](public/index.html) — no build step. Open the
file in a browser, or:

```sh
npx wrangler dev
```

## Deploy

Served as a Cloudflare Worker with static assets (`wrangler.jsonc`), with
`color.haxxor.xyz` as a custom domain:

```sh
npx wrangler deploy
```
