# Site plan & sync rules

Live site: https://wkotlewski.github.io/  
Repo: https://github.com/wkotlewski/wkotlewski.github.io

This file is the source of truth for how the site is built, published, and kept in sync — including when you only have GitHub access (no local `Documents/Resume` folder).

---

## What this site is

Single-page leadership impact site for Whitney Kotlewski:

- Dark theme landing page (`index.html`)
- Company cards (Slingshot, Matterport, Esri, Sonara) with expandable impact detail
- Headshot + product thumbnails
- “Download resume” opens the PDF in a **new tab**
- Links: Publications (Google Doc), LinkedIn, Contact

---

## Published file set (keep this lean)

Only these belong on GitHub Pages:

```
index.html
Whitney_Kotlewski_Resume_VP_Product.pdf
assets/whitney-headshot.jpg
assets/thumb-slingshot.jpg
assets/thumb-matterport.jpg
assets/thumb-esri.jpg
assets/thumb-sonara.jpg
README.md
SITE_PLAN.md          ← this file
```

Do **not** publish resume drafts, LinkedIn notes, Python build scripts, or large unoptimized PNG originals unless you intentionally want them public.

---

## Sync rule (local ↔ GitHub)

**Always update both places with the same files.**

1. Edit the site files (locally or on GitHub).
2. Deploy the **same** `index.html`, `assets/*`, and resume PDF to this repo’s `master` branch.
3. GitHub Pages builds from `master` / root → https://wkotlewski.github.io/

If local and GitHub diverge, treat the version you just edited as source of truth and push/copy it to the other side immediately.

### Local working copy (when available)

Typical path on Whitney’s Mac:

`/Users/whitney/Documents/Resume/`

That folder may contain extra private notes. Only the published file set above should be pushed to this repo.

---

## How to edit with only GitHub (no local files)

1. Open https://github.com/wkotlewski/wkotlewski.github.io
2. Edit `index.html` (pencil icon) → Commit to `master`
3. For images/PDF: **Add file → Upload files** into `assets/` or repo root
4. Wait ~30–60s for Pages to rebuild
5. Hard-refresh https://wkotlewski.github.io/

To confirm Pages settings: **Settings → Pages**

- Source: Deploy from branch
- Branch: `master` / `/ (root)`
- Site should be public (repo visibility: **Public** on free GitHub)

---

## Image performance rules

Images were slow because originals were too large (Esri thumb was ~2.7MB).

**Targets:**

| Asset | Role | Target |
|--------|------|--------|
| `whitney-headshot.jpg` | Hero (preload) | ~720px wide, ~40–60KB |
| `thumb-*.jpg` | Company cards | ≤960px wide, ~30–100KB each |
| Total images | Page weight | Prefer **&lt; 400KB** combined |

**Rules:**

- Prefer **JPEG** (or WebP) for photos/UI shots — not huge PNGs
- Keep `loading="lazy"` + `decoding="async"` on below-fold thumbs
- Keep `fetchpriority="high"` + `<link rel="preload">` on the headshot
- Resize before upload; don’t ship 2K+ screenshots for card thumbnails

---

## Resume download behavior

In `index.html`, the primary CTA should look like:

```html
<a class="btn btn-primary"
   href="Whitney_Kotlewski_Resume_VP_Product.pdf"
   target="_blank"
   rel="noopener noreferrer">
  Download resume …
</a>
```

- Opens PDF in a **new tab** (no `download` attribute)
- PDF must live at repo root with that filename

When replacing the resume: upload the new PDF with the **same filename**, or update both the file and the `href`.

---

## Visibility / privacy notes

- Repo is **public** so free GitHub Pages can serve the site.
- Anyone can view repo files and also download assets from the live page (View Source / Network).
- To hide the repo but keep a public site: GitHub Pro + private repo + Pages, or host from a private repo on Netlify / Cloudflare Pages / Vercel.

---

## Deploy checklist (every change)

- [ ] Edit `index.html` / assets / PDF
- [ ] Keep local copy updated (if you have it)
- [ ] Push same files to `master` on this repo
- [ ] Confirm Pages build succeeded (Settings → Pages, or Actions)
- [ ] Hard-refresh https://wkotlewski.github.io/ and spot-check images + resume link

---

## Quick CLI deploy (when local access exists)

From a folder that contains the published file set:

```bash
# Example: copy site files into a clone, then
git add index.html assets Whitney_Kotlewski_Resume_VP_Product.pdf README.md SITE_PLAN.md
git commit -m "Update site"
git push origin master
```

Auth: GitHub CLI (`gh auth login`) or HTTPS/SSH credentials for `wkotlewski`.

---

## History / decisions worth remembering

- Replaced the old multi-page portfolio with this single leadership-impact page (Jul 2026).
- Made the repo public and enabled Pages from `master` `/`.
- Optimized images after slow loads (PNG → compressed JPEG).
- Agreement: every fix updates **local + GitHub** so they stay in sync.
- This plan file lives in the repo so it remains available without local disk access.
