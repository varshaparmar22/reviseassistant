# Publishing your Revise Assistant

You have two identical files:
- **`index.html`** — use this for hosting (hosts look for `index.html` by default).
- **`revise-assistant.html`** — same deck, handy for opening locally or emailing to yourself.

Pick **one** route below. All three give you a private-by-default, HTTPS link.

---

## Privacy — read first (30 seconds)
This deck contains your personal interview prep. It's already set to **`noindex`**, so Google/Bing won't list it — but anyone with the exact link can still open it. To lock it down more:

- **Passphrase lock (built in):** open the file, find `const PASSPHRASE = "";` near the bottom, and change it to a word, e.g. `const PASSPHRASE = "varsha2026";`. Visitors must type it to see the cards. *(Light protection — good enough to keep casual eyes out, not bank-grade security.)*
- **Unguessable path:** host it at a weird path like `/revise-x8f2/` instead of `/revise/`.
- **Host-level password:** some hosts (e.g. Cloudflare Access, DanubeData) offer real password protection — use that if you want strong privacy.

---

## Route 1 — Your own AWS pipeline (recommended, zero new setup)
You already built GitHub → CodePipeline → S3 → CloudFront with automatic cache invalidation. Reuse it — and it doubles as a talking point ("I host my own tools on my own CI/CD pipeline").

**Option A — through Git (auto-deploy):**
1. Copy `index.html` into your portfolio repo, e.g. into a `revise/` folder so it lives at `/revise/`.
2. `git add revise/index.html && git commit -m "Add revise assistant" && git push origin main`.
3. CodePipeline deploys to S3 → EventBridge fires your Lambda → CloudFront cache is invalidated.
4. Live at `https://<your-cloudfront-domain>/revise/`.

**Option B — direct upload (fastest):**
```bash
aws s3 cp index.html s3://<your-bucket>/revise/index.html
aws cloudfront create-invalidation --distribution-id <YOUR_DIST_ID> --paths "/revise/*"
```

> Note: entering your AWS credentials and running these commands is something to do yourself in your own terminal — don't paste secrets anywhere you don't control.

---

## Route 2 — Netlify Drop (fastest overall, ~60 seconds, no Git)
1. Go to **app.netlify.com/drop**.
2. Drag **`index.html`** (or a folder containing it) onto the page.
3. You get an instant live URL. Sign in (free) to keep it and rename the site.
4. Optional: add a custom domain in Site settings.

Free tier is credit-based (~30 GB bandwidth and ~20 deploys/month) — plenty for a personal study tool.

---

## Route 3 — GitHub Pages (free, version-controlled)
1. Create a new repo (e.g. `revise-assistant`) and add **`index.html`** to it.
2. Repo → **Settings → Pages** → Source: **Deploy from a branch** → `main` / root → Save.
3. In ~1 minute it's live at `https://<your-username>.github.io/revise-assistant/`.

> Heads-up: a **public** repo means the file is publicly viewable in the repo too. For a private repo with Pages you need GitHub Pro. If privacy matters, prefer Route 1 or turn on the passphrase lock.

---

## Custom domain (optional, any route)
Buy a domain (Namecheap, Cloudflare, etc.), then point a DNS record to your host and let the host issue the free SSL certificate. Example: `revise.varshaparmar.dev`.

## Updating later
Re-export the deck, replace `index.html`, and redeploy (push to Git, re-drag to Netlify, or re-run the S3 copy + CloudFront invalidation). Your saved *Known/Review* progress lives in each visitor's browser, so it persists across updates on the same device.
