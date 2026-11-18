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

## Deploying this repo (GitHub Pages)

The included workflow (`.github/workflows/deploy.yml`) deploys this folder
to GitHub Pages on every push to `main` — no build step, because there's
nothing to build. To go live:

1. Push this repo to GitHub.
2. In the repo's **Settings → Pages**, set **Source** to **GitHub
   Actions** (the workflow will then run automatically; it already ran
   once the moment you pushed, so you may just need to re-run it from the
   **Actions** tab after making this change).
3. Point your DNS at GitHub Pages for this subdomain. For a subdomain
   (`sastore.cungusportracing.com`) add a **CNAME record**:
   `sastore` → `<your-github-username-or-org>.github.io`
   (GitHub's own docs: <https://docs.github.com/pages/configuring-a-custom-domain-for-your-github-pages-site>)
4. Back in **Settings → Pages**, under **Custom domain**, enter
   `sastore.cungusportracing.com` and save (this repo already ships a
   `CNAME` file with that value, which the workflow publishes automatically
   — the Settings field just needs to match it once DNS is live, so GitHub
   can provision HTTPS for it).
5. Wait for DNS to propagate and for GitHub to issue the certificate (shows
   as a green check in Settings → Pages once ready), then enable **Enforce
   HTTPS**.

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
