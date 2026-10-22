# ogood-landing

Landing site for [ogood.network](https://ogood.network), the home of Ogood: an open federation protocol for volunteer networks (VolunteerMatch × ActivityPub × Tinder, built on the Volunteering Data Standard).

The site is a Cloudflare Worker with static assets. The Worker in `src/index.js` hands every request to the `ASSETS` binding, which serves the HTML files from the repo root.

## Layout

```
ogood-landing/
├── wrangler.toml        # Worker config; [assets] serves the repo root
├── .assetsignore        # keeps repo-only files (src/, README, config) out of the public assets
├── src/
│   └── index.js         # Worker entry: passes requests to env.ASSETS
├── index.html           # landing page, served at /
└── architecture.html    # proposed Cloudflare architecture, served at /architecture.html
```

## Local preview

```bash
npx wrangler dev
```

Then open http://localhost:8787. Any static file server also works, since the pages are plain HTML (`python3 -m http.server`).

## Deploy

You need a Cloudflare account with access to the `ogood.network` zone.

```bash
npm install -g wrangler        # or use npx wrangler for each command
wrangler login                 # opens a browser to authorize
wrangler deploy                # publishes the ogood-landing Worker
```

The custom domain only needs attaching once. Either add it in the Cloudflare dashboard (Workers & Pages → ogood-landing → Settings → Domains & Routes → Add → Custom domain → `ogood.network`), or add this to `wrangler.toml` and redeploy:

```toml
routes = [
  { pattern = "ogood.network", custom_domain = true }
]
```

After a deploy, check https://ogood.network/ and https://ogood.network/architecture.html.

## Adding pages

Drop a new `.html` file in the repo root and deploy; it's served at `/<name>.html`. If you add files that should not be public, list them in `.assetsignore`.
