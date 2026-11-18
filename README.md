# CSR Kart Parts — South Africa storefront (sastore)

This is the whole repo: `index.html` loads `store-app.js` from Team App's
own domain (`https://app.cungusportracing.com/store-app.js`), and that one
script builds the entire storefront — header, category menu, home page,
category/product pages, the "My list" drawer and the quote form — reading
its own API origin off its own `<script src>` tag. There is nothing else
to build here; this repo exists to serve one static HTML file at
`sastore.cungusportracing.com`.

See `team-app`'s own repo — specifically
`claude/session-2026-11-18-storeadmin-integration.md` — for the full
history of why store-app.js lives there instead of here: it used to be
served from a separate backend (`storeadmin.cungusportracing.com`), which
is being retired in favour of Team App's own Firebase project.

## Deploying this repo (Netlify, connected to this GitHub repo)

No build step — this repo IS the published output, so Netlify just serves
it as-is on every push. To go live:

1. Go to <https://app.netlify.com> and log in (or create a free account).
2. **Add new site → Import an existing project → Deploy with GitHub** and
   pick this repo (`sastore`).
3. Netlify reads `netlify.toml` at the repo root and deploys immediately —
   nothing else to configure. Every future push to `main` redeploys
   automatically, and every pull request gets its own disposable
   deploy-preview URL for free.
4. Point your custom domain (`sastore.cungusportracing.com`) at this site
   from **Site configuration → Domain management** in the same dashboard —
   Netlify will tell you the DNS record to add at your registrar and
   provision HTTPS for it automatically once DNS is live.

## Changing anything here

There's really only one thing to change in this repo: `index.html`'s
`<meta>` tags (title, description, Open Graph) if the storefront's own
copy ever needs editing. Everything about how the catalogue actually
looks and behaves — pricing, categories, the quote form, styling — lives
in `store-app.js` in the `team-app` repo, not here.

## If store-app.js's domain ever changes again

Update the single `<script src="...">` line in `index.html` (and the
matching `og:image` URL, which currently points at the same domain's
`/brand/` folder for the logo) — that is the entire client-side cutover,
by design. No other file in this repo references that domain.
