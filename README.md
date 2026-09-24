# Kepler — holding page

A single static page for `keplerquant.com` while the real site is built.
One HTML file plus brand assets. No build step, no framework, no API.

## Deploying to Amplify

Amplify deploys from a git repository, so:

```bash
git init
git add .
git commit -m "Kepler holding page"
git remote add origin git@github.com:<you>/kepler-holding.git
git push -u origin main
```

Then in the Amplify console: **New app → Host web app → GitHub →** pick the
repo. It will detect `amplify.yml`, which has an empty build phase and
publishes the folder as-is.

Add `keplerquant.com` under **Domain management**. Amplify issues the TLS
certificate; point the domain's nameservers (or a CNAME/ALIAS for the apex)
at the value Amplify gives you.

## Keep this repo separate from the product

The Next app cannot deploy from this config. It needs `npm ci && npm run
build`, a Node runtime, and a reachable API — its landing page is
server-rendered against `INTERNAL_API_URL` to draw the live record tiles and
equity curve. Deploy that as its own Amplify app when the FastAPI backend is
hosted, then repoint the domain.

## On launch day

1. Repoint `keplerquant.com` at the Next app.
2. Delete this app, or leave it on a subdomain as a status page.

## Two things to know

**The page is `noindex`.** Letting Google index a placeholder means the
brand's first impression in search is "coming soon", and the real page then
has to displace it. It is one `<meta name="robots">` line in `index.html` —
remove it only if you decide you want the placeholder indexed.

**No email capture.** A form needs somewhere to send the address, which means
a backend, a database and GDPR consent handling — for a page that exists for
a few weeks. The mailto link does the same job with none of that. If you want
a real list, use a hosted form (Buttondown, ConvertKit, Mailchimp) and drop in
its embed; that keeps the consent and storage obligations with them.
