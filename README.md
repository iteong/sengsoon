# Seng Soon Electrical Engineering: website

Company website for **Seng Soon Electrical Engineering Pte Ltd** (成順電器工程私人有限公司), a licensed electrical contractor in Singapore since 1971.

Live site: https://www.sengsoon.com.sg/

It's a static, single-page site with no framework and no build step. The site is aimed at two audiences: **clients** (main contractors, M&E firms, MCSTs, rail and building owners) and **job seekers**.

---

## Repository layout

```
README.md          this file (not published on the website)
vercel.json        caching and security headers
public/            everything the website serves
  index.html       the whole site: content, styles and scripts
  img/             optimised photos and logo
  404.html         "No power here." page for broken links
  robots.txt       crawler rules (search engines and AI assistants allowed)
  sitemap.xml      for Google and Bing
  llms.txt         plain-text company summary for AI assistants
  og-image.png     preview image shown when the link is shared
  favicon.ico, favicon-32.png, apple-touch-icon.png, icon-192.png, icon-512.png
  site.webmanifest
```

Only `public/` is served, so files at the repository root (like this README) don't appear on the website.

---

## Deploying on Vercel

1. Vercel › **Add New › Project** › import this repository.
2. Set Framework Preset to **Other**, leave Build Command empty, and set Output Directory to **`public`**.
3. Deploy. Every later push to the main branch redeploys automatically.
4. Settings › Domains: add `www.sengsoon.com.sg` as the primary domain, and `sengsoon.com.sg` redirecting to www.
5. Set the DNS records Vercel shows: the A record for `@` and the CNAME for `www`. **Only change these two.** Leave MX, SPF, DKIM and the other email records alone, or company email will stop working.

After launch:
- **Google Search Console:** verify the domain and submit `https://www.sengsoon.com.sg/sitemap.xml`
- **Bing Webmaster Tools:** import from Search Console
- **Google Business Profile:** keep the name, address, phone and hours identical to the site

---

## Page sections

Header, then: hero (animated project map), credentials, key figures, who we serve, services (single-line diagram), projects (photo cards plus a filterable list), about, careers, common questions, contact, closing banner.

## Design

| | |
|---|---|
| Colours | Night navy `#0A0E2C`, Seng Soon red `#E1251B` / `#FF4433`, paper `#F3F2EE` |
| Type | Archivo (Google Fonts) for everything; Noto Serif TC for 成順電器 |
| Layout | Light and dark mode supported; works from 320px phones to wide desktops |

**Animations** (all pause when off screen, replay when scrolled back into view, and are disabled for visitors with "reduce motion" turned on):
- **Hero map:** the island is drawn as dots, and project lights switch on in date order. Live projects pulse, and landmark projects are labelled.
- **Headings:** they rise in word by word.
- **Key figures:** they count up from zero.
- **Services:** the busbar energises, the circuits light in turn, then a current pulse loops.
- **Closing banner:** a neon-sign flicker.

## Performance and search

- **Images:** WebP with a JPG fallback, in two sizes, lazy loaded. File names include a content hash, so they can be cached for a year.
- **First load on a phone:** about 118 KB, with content visible in under half a second and no layout shift.
- **Structured data** (schema.org JSON-LD) in `index.html`: Organization/Electrician (contact, address, hours, credentials, services), FAQPage, and an ItemList of projects.
- **Works without JavaScript.** All content, including the full project list, is in the HTML.

---

## Updating the site

All content lives in `public/index.html`. **Some information appears in more than one place, so update every copy.**

### Projects
Each project is one `<tr>` row in the project table:

```html
<tr data-s="residential" data-ll="103.8690,1.3850" data-y="2025" data-live="1" data-label="Luxus Hill">
  <td>2025 – <span class="pill on">Live</span></td>
  <td><b>Project name</b><span class="sc">Scope of works</span></td>
  <td class="cl">Client</td>
  <td class="sector">Residential</td>
</tr>
```

| Attribute | Meaning |
|---|---|
| `data-s` | Filter group: `residential`, `healthcare`, `commercial`, `rail` or `industrial` |
| `data-ll` | Map position as `longitude,latitude` (leave it out for no map light) |
| `data-y` | The year the light switches on in the map animation |
| `data-live="1"` | Marks a current project (pulsing light) |
| `data-label` | Shows a name next to the light on the map (use sparingly) |

The on-page counts ("Show all N projects", "Showing N recent and notable projects") update automatically. **These do not, so update them too:**
- the `ItemList` structured data near the top of `index.html`
- the "Selected projects" section in `public/llms.txt`

### FAQ
If you edit a question or answer, also update the `FAQPage` structured data and the FAQ section of `llms.txt`.

### Phone, email, address, hours
These appear in the contact section, the structured data, `llms.txt`, `og-image.png` and the Google Business Profile. Keep them all identical.

### Images
- Export WebP (about 960px wide, quality around 70) and a JPG fallback (about 800px).
- Use a **new file name** for every new or changed image (for example `name.<hash>-960.webp`). Images are cached for a year, so a reused name won't refresh.
- Always include `width`, `height` and descriptive `alt` text.

### Domain
If the site moves to a different address, search and replace `https://www.sengsoon.com.sg/` in `index.html`, `robots.txt`, `sitemap.xml` and `llms.txt`.

### Before each publish
- Check on a phone that nothing scrolls sideways.
- Run Google's [Rich Results Test](https://search.google.com/test/rich-results) on the live page to check the structured data.

---

© Seng Soon Electrical Engineering Pte Ltd · UEN 198804759R
