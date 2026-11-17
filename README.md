# cigarbox.bm: Cuarenta Bucaneros website

A plain static website: HTML, one CSS file, and images. There's no JavaScript, no web fonts, no analytics, no forms, no plugins and no third-party requests.

## What's in here

| File | Purpose |
|---|---|
| `index.html` | Home page: how to order, brands overview, visit info |
| `cigars.html` | Brands & sizes we *typically* carry (no prices, no live stock) |
| `visit.html` | Address, hours, phone, FAQ |
| `404.html` | Shown for any old/broken link (e.g. old shop pages). Uses `/`-rooted paths so it works at any URL depth on Pages, which means it looks unstyled if opened directly from disk. |
| `css/styles.css` | All styling |
| `images/` | Photos (EXIF/GPS metadata stripped), the official logo (`cuarenta-bucaneros-logo*.png`, extracted from the business-card artwork) and favicons |
| `CNAME` | Tells GitHub Pages to serve the site on `cigarbox.bm` |
| `.nojekyll` | Tells GitHub Pages to serve the files as-is |
| `robots.txt`, `sitemap.xml` | Search engine housekeeping |

## Security notes

- Every page carries a Content-Security-Policy meta tag: `default-src 'none'; img-src 'self'; style-src 'self'; base-uri 'none'; form-action 'none'`. The browser will refuse to run any script or load anything from another site, even if someone injected markup.
- The only outbound links are `tel:` links and one "Open in Maps" link (a plain link, not an embedded map).
- GitHub Pages can't set custom HTTP headers, so `X-Frame-Options`/`frame-ancestors` aren't available. That's an acceptable gap for a brochure site with no logins or forms.
- **Do not** upload the old backup folders (`Storefront/`, etc.) to the repo. They contain the old card-payment order form and ShopFactory `.cgi` scripts.

## Publishing on GitHub Pages (free tier)

> Pages on a free GitHub account needs a **public** repository. Nothing in this site is secret, so that's fine.

1. Create a new public repository, e.g. `cigarbox-site`.
2. Upload the *contents* of this folder to the root of the repo (including `CNAME` and `.nojekyll`).
3. Go to **Settings → Pages**. Under *Build and deployment*, set Source to **Deploy from a branch**, Branch to `main`, and folder to `/ (root)`.
4. **Verify the domain first** (this protects against domain takeover). Go to your GitHub profile **Settings → Pages → Add a domain**, enter `cigarbox.bm`, and add the TXT record GitHub gives you at the .bm DNS provider.
5. At the DNS provider for `cigarbox.bm`, set:
   - `A` records for `cigarbox.bm` → `185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153`
   - `AAAA` records (optional) → `2606:50c0:8000::153`, `2606:50c0:8001::153`, `2606:50c0:8002::153`, `2606:50c0:8003::153`
   - `CNAME` for `www` → `<your-github-username>.github.io`
   - Remove any old A/CNAME records pointing to the previous host.
6. Back in the repo's **Settings → Pages**, confirm the custom domain shows `cigarbox.bm`. Once the DNS check passes, tick **Enforce HTTPS**. The certificate can take up to an hour or so.

Check GitHub's current docs ("Managing a custom domain for your GitHub Pages site") in case the IP addresses have changed.

## Brand colours

The palette in `css/styles.css` (`:root`) is sampled from the logo: black `#231f20`, rust `#a13d0f`, orange `#ef9f3a`, peach `#f5caa1`, and the business card's pale aqua `#e4f1ef`.

## Making common edits

You can edit the files directly on GitHub (click the file, then the pencil icon). Changes go live in a minute or two.

- **Phone number / hours / address** appear on every page (header, footer and visit blocks). Use find-and-replace across `index.html`, `cigars.html`, `visit.html` and `404.html`. The phone number appears in two formats: `+1 (441) 295 4523` (displayed) and `tel:+14412954523` (the tap-to-call link).
- **Email** (`cigars@cigarbox.bm`) is written as HTML character codes (`&#99;&#105;...`) so that simple spam bots scraping the page don't pick it up. It displays normally. To change it, replace the whole encoded string, or just type the new address in plain text.
- **Brands & cigars:** in `cigars.html`, each brand is one block starting with a comment like `<!-- ===== Cohiba ===== -->`. Edit the cigars inside `<ul class="sizes">`, or delete the whole `<article>…</article>` to remove a brand. Strength dots are set by `s1`–`s5` on the `dots` span.
- **Photos:** put a new JPG in `images/` (around 900px wide is plenty) and change the `src`, `width`, `height` and `alt` on the `<img>` tag.

### Cigar photo previews (Our Cigars page)

The previews are plain HTML and CSS; there's no script. Images live in:

- `images/cigars/`: one photo per cigar, named `Brand-Size-extras.jpg` (e.g. `Cohiba-Siglo_I.jpg`, `Partagas-Coronas_Senior-tubos.jpg`)
- `images/brands/`: one band/logo per brand (e.g. `Cohiba.jpg`)
- `images/boxes/`: the box and lifestyle shots in the "From the humidor over the years" grid

**To add a cigar *with* a photo,** copy an existing chip from `cigars.html` and change the text and file name:

```html
<li class="has-photo" tabindex="0">Siglo III<span class="peek" role="tooltip"><img src="images/cigars/Cohiba-Siglo_III.jpg" alt="Cohiba Siglo III" loading="lazy"><span>Cohiba Siglo III</span></span></li>
```

**A cigar *without* a photo** is just `<li>Siglo III</li>`. About 70 spare cigar photos that aren't on the site yet are kept on the computer in `new-site/images/cigars/` (listed in `missing-cigar-photos.csv` in the images-to-confirm folder). To use one, upload it to `images/cigars/` on GitHub and add its chip.

New cigar photos: crop tight, lay the cigar horizontally on a white background, ~600px wide, saved as JPG.

## Preview locally

Open `index.html` in any browser. No build step or server is needed.
