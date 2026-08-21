# Pollen Image Studio

**Pollen Image Studio** is a single-file web app for generating images with the
[Pollinations AI](https://pollinations.ai) image API, using the official
**BYOP (Bring Your Own Pollen)** OAuth flow — you sign in with your own
Pollinations account and spend **your own Pollen credits**.

> Live demo: https://image.lanprint.com/

---

## Features

- **Multiple models** — Flux, ZImage, Dreamshaper, GPT-Image, GPT-Image-2,
  Klein, Kontext, Nova-Canvas, plus arbitrary custom model IDs
- **12 aspect-ratio sizes** — square, 3:4 / 4:3, 2:3 / 3:2, 16:9 / 9:16
- **Built-in reference prompts** — 6 curated example cards + a random prompt
  generator, each with native Chinese and English versions
- **Official BYOP authorization** — OAuth 2.0 Authorization Code flow + PKCE
  against `enter.pollinations.ai`; the scoped `sk_` key is kept only in
  `sessionStorage` and can be revoked at any time
- **Bilingual UI** — Chinese / English toggle (persisted in `localStorage`)
- **Automatic prompt translation** — when the user's region is China and the
  prompt contains Chinese, it is translated to English for better results
  using Pollinations' free text endpoint (no Pollen cost); the original and
  the translation are both shown in the result metadata

---

## How it works

1. Open the app and click **Authorize & Connect** — it starts the official
   BYOP OAuth flow; you approve via your Pollinations / GitHub account.
2. The app exchanges the authorization code (with PKCE) for a scoped `sk_`
   key and stores it for the current session only.
3. Type a prompt (or use a reference card / random prompt), pick a model and
   a size, then click **Generate**.
4. The image is requested from `POST https://gen.pollinations.ai/v1/images/generations`
   and rendered inline, with download and regenerate actions.

There is no backend — everything runs in your browser. The repo is a single
`index.html` with no build step, hosted as a static site.

---

## Quick start (local)

```bash
# clone
git clone https://github.com/dokirong/pollen-image-studio.git
cd pollen-image-studio

# just open it
# (any static server works too)
```

Open `index.html` in a browser. No dependencies to install.

---

## BYOP: App Key & Redirect URI

- The app embeds a Pollinations **App Key** (`pk_...`) as its OAuth
  `client_id`. A `pk_` key is publishable by design and safe to ship in
  frontend code.
- The **Redirect URI** registered for the App Key must exactly match:

  ```
  https://image.lanprint.com/
  ```

- Scoped `sk_` keys are never stored in source, URL, or localStorage.

---

## Models & displayed pricing

Price shown on each model card is an *estimate per image* (frontend display
only; the actual billing is performed by Pollinations servers).

| Model        | Displayed price  |
|--------------|------------------|
| dreamshaper  | 0.0001 pollen    |
| flux         | 0.002 pollen     |
| zimage       | 0.004 pollen     |
| klein        | 0.005 pollen     |
| gptimage     | 0.02 pollen      |
| gpt-image-2  | 0.08 pollen      |
| kontext      | 0.04 pollen      |
| nova-canvas  | 0.04 pollen      |
| custom       | model-defined    |

---

## Deployment

The app is published on GitHub Pages from the `main` branch (repository root,
`index.html`). After pushing, Pages rebuilds automatically; CDN caches may
take a few minutes.

---

## Disclaimer

- Generated images are AI-created — please respect content policies and cite
  or verify before commercial use.
- In case of `429` / `503`, wait per `Retry-After` and try again.
- This project is provided as-is for personal and educational use.
