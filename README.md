# Seng Soon Electrical Engineering: website

Company website for **Seng Soon Electrical Engineering Pte Ltd** (成順電器工程私人有限公司), a licensed electrical contractor in Singapore since 1971.

Live site: https://www.sengsoon.com.sg/ (also at https://sengsoon.vercel.app/)

It's a static, single-page site with no framework and no build step. The site is aimed at **clients**: main contractors, M&E firms, MCSTs, rail and building owners. The careers section and the Assistant Engineer job card were removed in October 2026 because the company isn't hiring for now; they're in git history (the commit before "Remove the careers section") if hiring resumes.

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

All five steps are done. Techclique switched the DNS on 16 Oct 2026, and the site has been live on `www.sengsoon.com.sg` since then. The records are:

| Type | Name | Value |
|---|---|---|
| A | `@` | `216.198.79.1` |
| CNAME | `www` | `610f04facff5b299.vercel-dns-017.com` |

Both domains show **Valid Configuration** under Vercel › Settings › Domains.

### Domain and email
- **DNS** is hosted by Techclique (Vincent Chan, vincentchan@techclique.com.sg), who also run company email. Ask them for DNS changes. The registry delegates to `ns811/ns812.sgcloudhosting.com`, the same server as `ns1/ns2.techclique.com.sg`.
- **Email** runs on the old server: the `mail` A record (`103.7.8.33`), the MX record and the SPF record all point there. Never change or remove them.
- **Google verification:** a TXT record on `@` starting `google-site-verification=` proves ownership to Google Search Console. It sits alongside the SPF TXT record. Never remove it, or access to Search Console is lost.
- **Renewal:** the `.com.sg` domain is registered through GoCloudEasy, with Techclique managing renewals for S$60 a year. It expires on **30 November** each year; if it lapses, the website and company email both stop working. Techclique renewed it on 16 Oct 2026 for 30 Nov 2026 to 29 Nov 2027 (invoice TCINV01210). As of 18 Oct 2026, `whois sengsoon.com.sg` still shows the old expiry date (2026-11-30). Check that it shows 2027-11-30 before 30 Nov 2026.
- **Registrant:** Seng Soon Electrical Engineering Pte Ltd, which is also the administrative contact (admin@sengsoon.com.sg). The registrant was transferred from Techclique Pte Ltd on 16 Oct 2026, and the details were verified on SGNIC's [VerifiedID@SG](https://verifiedid.sgnic.sg/) through Singpass the same day. GoCloudEasy is the technical contact.

After launch:
- [x] **Google Search Console:** Domain property `sengsoon.com.sg`, verified by DNS. Sitemap `https://www.sengsoon.com.sg/sitemap.xml` submitted and homepage indexing requested.
- [x] **Bing Webmaster Tools:** imported from Search Console, with the sitemap submitted separately (the import didn't bring it across) and homepage indexing requested.
- [ ] **Google Business Profile:** keep the name, address, phone and hours identical to the site. The site links to the existing Maps listing, but the listing's website field still points to bca.gov.sg and it has no opening hours. Whoever owns the listing needs to fix both.

### Analytics
- **Amplitude** (project `sengsoon`): analytics with autocapture and Session Replay, loaded from jsDelivr in the `<head>` of `index.html`. It only starts on `www.sengsoon.com.sg`, `sengsoon.com.sg`, `sengsoon.vercel.app` and `localhost`, so Vercel's screenshot bot on per-deployment URLs isn't counted. The API key in the page is public by design; never put a secret key there.
- **Vercel Web Analytics:** visitor and page-view counts, served from the site's own domain so ad blockers rarely block it. Enabled under Vercel › Analytics.

---

## Page sections

Header (the menu collapses to a ☰ button below 900px wide), then: hero (animated project map), credentials, key figures, who we serve, services (single-line diagram), projects (photo cards plus a filterable list), about, common questions, contact, closing banner.

## Design

| | |
|---|---|
| Colours | Night navy `#0A0E2C`, Seng Soon red `#E1251B` / `#FF4433`, paper `#F3F2EE` |
| Type | Archivo (Google Fonts) for everything; Noto Serif TC for 成順電器 |
| Layout | Light and dark mode supported; works from 320px phones to wide desktops |

**Animations** (all pause when off screen, replay when scrolled back into view, and are disabled for visitors with "reduce motion" turned on):
- **Hero map:** the island is drawn as dots, and project lights switch on in date order. Live projects pulse, and landmark projects are labelled. Hovering a light (or tapping it on a phone) shows the project.
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

### Credentials
BCA registration, the LEW, bizSAFE, ISO certificates and SMRT appear in the credentials strip, the "Is Seng Soon a registered electrical contractor?" FAQ answer, the `hasCredential` structured data and `llms.txt`. Update all four. Only use official logo files supplied by each certifying body, and only while the certificate is valid.

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
- Check the structured data on the live page with the [Schema Markup Validator](https://validator.schema.org/) (no sign-in needed), or Google's [Rich Results Test](https://search.google.com/test/rich-results) while signed in to a Google account.

---

© Seng Soon Electrical Engineering Pte Ltd · UEN 198804759R
