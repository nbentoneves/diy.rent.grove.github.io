# Oxfordshire DIY Tool Hire – Hugo site

This is a Hugo website for a local DIY tool rental side hustle in Oxfordshire, United Kingdom.

The site is built on top of the [Hargo Hugo theme][hargo], consumed as a **Hugo Module**, and adapted for local lead generation instead of e‑commerce checkout.

[hargo]: https://github.com/gethugothemes/hargo-hugo

## Features

- Built with Hugo extended and the Hargo theme (via Hugo Modules)  
- Local‑SEO focused copy for Grove, Wantage, Abingdon, Oxford, Didcot and nearby villages  
- Equipment section with structured front matter for each tool (including Hygglo URL)  
- Category and location landing pages for internal linking and local relevance  
- JSON‑LD LocalBusiness and Product schema  
- FAQ page with FAQPage schema  
- Canonical URLs, Open Graph and Twitter meta tags  
- Clean URLs for equipment, blog posts, categories and locations  
- GitHub Actions workflow for GitHub Pages deployment

## Requirements

- [Hugo extended][hugo-install] (v0.120 or newer recommended)  
- Go installed (required for Hugo Modules)  
- Git for version control

[hugo-install]: https://gohugo.io/installation/

## Project structure

```text
oxon-tool-hire/
  archetypes/
    equipment.md          # Front‑matter template for new tools
  content/
    _index.md             # Home page
    equipment/            # All tool pages
    categories/           # Category landing pages
    locations/            # Location landing pages
    about/_index.md       # About page
    contact/_index.md     # Contact page
    faq/_index.md         # FAQ with schema data
    blog/                 # Blog index and posts
  layouts/
    index.html            # Custom home page
    equipment/            # List/single templates for tools
    locations/            # List/single templates for locations
    faq/list.html         # FAQ page layout
    partials/             # Header, SEO, schema, breadcrumbs, Hygglo CTA
  static/
    images/equipment/     # Equipment images (add your own)
    images/branding/      # Logo and favicon
  .github/workflows/
    hugo-gh-pages.yml     # GitHub Actions workflow for Pages
  hugo.toml               # Main configuration (TOML)
  README.md
```

## Hugo Module setup (Hargo theme)

The site imports the Hargo theme as a Hugo Module via the `module.imports` block in `hugo.toml`.

```toml
[module]
  [[module.imports]]
    path = "github.com/gethugothemes/hargo-hugo"
```

To initialise the module locally:

1. Make sure you are in the project root:

   ```bash
   cd oxon-tool-hire
   ```

2. Initialise the site as a Hugo module (creates `go.mod`):

   ```bash
   hugo mod init github.com/your-github-username/oxon-tool-hire
   ```

3. Fetch the Hargo theme module:

   ```bash
   hugo mod get github.com/gethugothemes/hargo-hugo
   ```

This is the standard way to use a theme via Hugo Modules rather than the older `themes/` directory mechanism.[cite:23][cite:26]

### Why this works (module vs legacy theme)

The Hargo repository is a regular Hugo theme with `layouts`, `assets` and `static` directories, so it can be consumed directly as a Hugo Module without any extra changes.[cite:1][cite:26]

No files are copied into `themes/`; instead Hugo pulls the theme at build time using the module system, and any templates placed in this project’s `layouts/` directory override the theme defaults where needed.

## Local development

After completing the module setup above:

```bash
cd oxon-tool-hire
hugo server -D
```

Then open the local URL printed in the terminal (usually `http://localhost:1313`) to preview the site.[cite:14]

## GitHub repository setup

1. Create a new repository on GitHub, for example `oxon-tool-hire`.  
2. Initialise Git and push the project:

   ```bash
   cd oxon-tool-hire
   git init
   git add .
   git commit -m "Initial Hugo site for Oxfordshire DIY Tool Hire"
   git branch -M main
   git remote add origin git@github.com:your-github-username/oxon-tool-hire.git
   git push -u origin main
   ```

3. Update `baseURL` and business URLs in `hugo.toml` to match your GitHub Pages URL, for example:

   ```toml
   baseURL = "https://your-github-username.github.io/oxon-tool-hire/"
   [params]
     businessUrl = "https://your-github-username.github.io/oxon-tool-hire/"
   ```

## Deploying to GitHub Pages with GitHub Actions

This project includes a workflow at `.github/workflows/hugo-gh-pages.yml` that builds the site with Hugo extended and deploys it to GitHub Pages.

High‑level flow:

- Install Hugo extended on an `ubuntu-latest` runner  
- Check out your repository  
- Run `hugo --gc --minify` to build into `public/`  
- Upload the `public/` folder as the Pages artifact  
- Deploy using the `deploy-pages` GitHub Action[cite:16][cite:22]

### One‑time configuration in GitHub

1. In **Settings ▸ Pages**, set **Source** to **GitHub Actions**.  
2. Commit and push your changes to `main`.  
3. The "Deploy Hugo site to Pages" workflow should run automatically – once it completes successfully, GitHub Pages will serve your site at `https://your-github-username.github.io/oxon-tool-hire/`.[cite:22]

## Adding or editing tools

Each equipment item lives in `content/equipment/` as a Markdown file with front matter.

Example (simplified):

```toml
+++
title = "Electric pressure washer"
slug = "electric-pressure-washer-oxfordshire"
categories = ["cleaning"]
locations = ["Grove","Wantage"]
shortDescription = "Compact but powerful washer for patios and driveways."
longDescription = "Longer SEO‑friendly description…"
dailyPrice = "£25"
image = "images/equipment/pressure-washer.jpg"
condition = "Regularly serviced."
availabilityNote = "Usually available for weekend hire."
serviceArea = ["Grove","Wantage","Abingdon"]
featured = true
hyggloUrl = "https://www.hygglo.co.uk/listing/placeholder-pressure-washer" # TODO: replace with your live listing URL
seoTitle = "Pressure Washer Hire in Oxfordshire"
metaDescription = "Hire an electric pressure washer in Oxfordshire…"
+++

Long‑form content here.
```

Key points:

- **Hygglo URL** is required on every equipment page – this is where bookings happen.  
- **`featured = true`** makes a tool appear in the "Popular this week" section on the home page.  
- **`categories`** and **`locations`** drive the category and location listings for internal linking and local SEO.

You can also use the `archetypes/equipment.md` template when creating new tools:

```bash
hugo new equipment/your-tool-slug.md
```

Hugo will pre‑populate the front matter with the correct fields.[cite:14]

## SEO implementation details

- **Unique titles & meta descriptions** – Every page sets `seoTitle` and `metaDescription` in front matter; the custom `head.html` partial uses these where present.  
- **Canonical tags** – `head.html` outputs a `<link rel="canonical">` tag based on the page permalink to avoid duplicate‑content issues.[cite:21]  
- **Open Graph & Twitter** – Basic OG and Twitter meta tags are generated using the title, description and either the page image or the site logo.  
- **LocalBusiness schema** – `schema-local-business.html` emits JSON‑LD `LocalBusiness` data on the home, locations, about and contact pages, populated from `hugo.toml` params.[cite:18]  
- **Product schema for tools** – `schema-equipment.html` outputs `Product` JSON‑LD on equipment pages using pricing, service area and Hygglo URL.  
- **FAQPage schema** – `schema-faq.html` generates `FAQPage` JSON‑LD from the `faqs` array defined in `content/faq/_index.md`.  
- **Breadcrumbs** – `breadcrumbs.html` builds simple breadcrumb navigation for equipment, blog, category and location pages.  
- **Robots & sitemap** – `enableRobotsTXT = true` is set in `hugo.toml`, and Hugo’s built‑in sitemap is enabled with a weekly change frequency.[cite:12][cite:14]

## Hygglo integration

Every equipment page uses the `hygglo-cta.html` partial to display clear booking calls‑to‑action:

- **Rent on Hygglo**  
- **Check availability on Hygglo**

Links open in a new tab with `rel="noopener noreferrer"` for safety. Replace the placeholder Hygglo URLs in each equipment file with your real listing URLs.

## Adapting Hargo from e‑commerce to local rental

Hargo is originally designed as a Snipcart‑enabled e‑commerce theme.[cite:1] For this site:

- The cart icon and checkout button in the header are removed via a custom `layouts/partials/header.html`.  
- Snipcart scripts are not loaded; the module import is only used for layout, styling and general structure.  
- Product pages are replaced by the `equipment` section, where the primary call‑to‑action is to visit Hygglo, not to add items to a cart.  
- Category, location and FAQ pages have been rewritten with local‑SEO‑friendly content for Oxfordshire.

## Image assets

Sample content assumes images such as:

- `static/images/equipment/pressure-washer.jpg`  
- `static/images/equipment/ladder.jpg`  
- `static/images/equipment/wallpaper-steamer.jpg`  
- `static/images/equipment/hedge-trimmer.jpg`  
- `static/images/equipment/sds-drill.jpg`

You can replace these with your own photos. Use descriptive file names and set meaningful `image` paths in front matter – Hugo’s templates will automatically set `alt` text based on the page title for basic accessibility.

## Notes

- This site is intentionally focused on **lead generation and local visibility**, not on‑site checkout.  
- Always keep Hygglo listing URLs and indicative pricing in sync between Hygglo and this site.  
- Update service areas, opening hours and contact details in `hugo.toml` as your coverage evolves.

