# Emiliana Parati — Decap CMS Setup

Your site is now content-managed: all text (AZ/EN/RU), the hero slideshow,
products, gallery, reviews, and brands live in JSON files under `/content/`
and are editable from a visual dashboard at `/admin`.

## What changed

```
index.html                 ← same design, now fetches content at runtime
admin/index.html            ← loads the Decap CMS app
admin/config.yml            ← defines every editable field
content/i18n/az.json        ← all AZ site text
content/i18n/en.json        ← all EN site text
content/i18n/ru.json        ← all RU site text
content/hero.json           ← hero slideshow images
content/products.json       ← 4 product cards (paint/wallpaper/doors/parquet)
content/gallery.json        ← 8 gallery tiles
content/reviews.json        ← 4 customer reviews
content/brands.json         ← 8 brand logos/names
content/settings.json       ← logo, address, phone, email, stats
uploads/                    ← where photos you upload in the CMS are stored
```

No build step is needed — it's still a static site, just data-driven.

## Step 1 — Put this in a GitHub repository

1. Create a new repository on GitHub (e.g. `emiliana-parati-site`).
2. Upload all the files/folders above to the repository root, keeping the
   folder structure exactly as-is.

## Step 2 — Deploy to Netlify

1. Go to [app.netlify.com](https://app.netlify.com) → **Add new site → Import an
   existing project**.
2. Connect GitHub and pick your repository.
3. Build settings: leave **Build command** empty and set **Publish directory**
   to `/` (the repo root) — there's nothing to compile.
4. Click **Deploy site**. Netlify will give you a URL like
   `https://your-site-name.netlify.app`.

## Step 3 — Turn on Netlify Identity + Git Gateway (this powers the login)

1. In your new site's Netlify dashboard: **Site configuration → Identity →
   Enable Identity**.
2. Under **Identity → Registration**, set it to **Invite only** (recommended,
   so random people can't create accounts).
3. Under **Identity → Services**, enable **Git Gateway**. This lets Decap CMS
   commit content changes back to your GitHub repo on your behalf, so your
   editors never need a GitHub account.
4. Under **Identity → Invite users**, invite yourself and anyone else who
   should be able to edit content. They'll get an email invite to set a
   password.

## Step 4 — Update two placeholders

In `admin/config.yml`, replace:
```yaml
site_url: https://your-site-name.netlify.app
```
with your actual Netlify URL, then commit/push the change.

If you already have a real `logo.png`, upload it into the `uploads/` folder
(or upload it via the CMS itself once you're in, under Site Settings → Logo).

## Step 5 — Access the dashboard

Go to:
```
https://your-site-name.netlify.app/admin
```
Log in with the Identity invite you accepted. You'll see these sections in
the sidebar:

- **Site Texts (AZ / EN / RU)** — every label, headline, and button, one
  editable form per language.
- **Hero Slideshow** — add/remove/reorder the rotating homepage photos.
- **Products** — edit the 4 product cards (name comes from Site Texts;
  description and button text are per-language here).
- **Gallery** — edit the 8 project photos, room tag, and captions; upload a
  real photo per tile (optional — if left empty it falls back to the
  existing textured placeholder look).
- **Reviews** — edit the 4 rotating customer quotes and author lines.
- **Brands** — edit the 8 brand names and which product category they
  belong to.
- **Site Settings** — logo, address, phone, email, map location, and the
  three stat numbers on the About section.

Every save commits directly to your GitHub repo; Netlify automatically
rebuilds and republishes the site (usually within ~30–60 seconds), since it's
just static files being updated.

## Notes

- The contact form and newsletter form are still front-end only (no email
  backend) — same as in the original file. If you want submissions to
  actually reach an inbox, that's a separate step (e.g. Netlify Forms) and
  I'm happy to wire that up on request.
- `hero_headline` and `hero_since` fields support basic HTML (e.g.
  `<em>word</em>` or `<br>`), since the design uses that for emphasis and
  line breaks — everything else is plain text.
- If you ever want to add a 5th product, gallery tile, review, or brand, use
  the **+ Add** button inside that collection in the CMS; no code changes
  needed.
