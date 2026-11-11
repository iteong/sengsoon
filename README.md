# Seng Soon Electrical Engineering: website

Company website for **Seng Soon Electrical Engineering Pte Ltd** (成順電器工程私人有限公司), a licensed electrical contractor in Singapore since 1973.

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
- [x] **Google Business Profile:** verified and managed by the company. The site links to it (`hasMap` in the structured data, and the map link in the contact section). Checked on 11 Nov 2026, it matches the site:
  - **Name:** Seng Soon Electrical Engineering Pte Ltd. **Category:** Electrician. **Service area:** Singapore.
  - **Address:** Kampong Ubi Industrial Estate, 3006 Ubi Road 1, #02-358, Singapore 408700.
  - **Phone:** 6742 3766. **Website:** https://www.sengsoon.com.sg/ (it used to point to bca.gov.sg). **Short name:** `sengsoonelectrical`.
  - **Hours:** Mon–Fri 10:00–18:00, Sat and Sun closed (no hours were set before).
  - **Description:** founded 1973 and incorporated 1988, BCA registered (ME05, L1), bizSAFE Star, ISO 9001 and ISO 45001, SMRT registered since 1998, the in-house LEW, the services (including main switchboard upgrades and EV chargers), and 200+ projects from condominiums and hospitals to MRT stations. If any of these change on the site, change the description too.
  - **Opening date:** 24 July 1973. **Logo:** the SE mark.
  - **Cover:** a 1920×1080 version of the 2026 letterhead, with everything stacked in a narrow centred column so the phone crop (about 4:5 from the middle) doesn't cut anything off. It lives next to the letterhead images, not in this repo. If you make a new one, keep the content within the middle ~860 px of the width.
  - **Social profiles:** LinkedIn and Facebook, the same pages the site links to.

  Remaining follow-up:
  - **Reviews:** 3 reviews (5.0) as of 11 Nov 2026. Ask happy MCST and contractor clients for more.

  Insights for Jun–Nov 2026: 974 profile views, 273 interactions, 34 website clicks and 13 calls. Several searches that showed the profile were for a different firm ("Huang Soon Electrical Engineering Works", 3026A Ubi Road), so some of the views come from people looking for that firm.

### Analytics
- **Amplitude** (project `sengsoon`): analytics with autocapture and Session Replay, loaded from jsDelivr in the `<head>` of `index.html`. It only starts on `www.sengsoon.com.sg`, `sengsoon.com.sg`, `sengsoon.vercel.app` and `localhost`, so Vercel's screenshot bot on per-deployment URLs isn't counted. The API key in the page is public by design; never put a secret key there.
- **Vercel Web Analytics:** visitor and page-view counts, served from the site's own domain so ad blockers rarely block it. Enabled under Vercel › Analytics.

---

## Page sections

Header (the menu collapses to a ☰ button below 900px wide), then: hero (animated project map), credentials, who we serve (with the key figures between its intro and the client groups), services (single-line diagram), projects (photo cards plus a filterable list), about, common questions, contact, closing banner.

## Design

| | |
|---|---|
| Colours | Night navy `#0A0E2C`, Seng Soon red `#E1251B` / `#FF4433`, paper `#F3F2EE` |
| Type | Archivo (Google Fonts) for everything; Noto Serif TC for 成順電器 |
| Layout | Light and dark mode supported; works from 320px phones to wide desktops |
| Credentials strip | Light background so the certification logos show in their original colours; navy in dark mode, with the logos on white tiles |

**Animations** (all pause when off screen, replay when scrolled back into view, and are disabled for visitors with "reduce motion" turned on):
- **Hero map:** the island is drawn as dots, and project lights switch on in date order. Live projects pulse, and landmark projects are labelled. Hovering a light (or tapping it on a phone) shows the project.
- **Headings:** they rise in word by word.
- **Key figures:** they count up from zero.
- **Services:** the busbar energises, the circuits light in turn, then a current pulse loops.
- **Closing banner:** a neon-sign flicker.

## Performance and search

- **Images:** WebP with a JPG fallback, in two sizes, lazy loaded. The certification logos are the exception: PNG only. File names include a content hash, so they can be cached for a year.
- **First load on a phone:** about 118 KB, with content visible in under half a second and no layout shift. The certification logos (about 155 KB) are lazy loaded, so on a phone they only load as the visitor scrolls to the credentials strip.
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

**Current certificates** (the PDFs and the logo files supplied by the certifiers are kept outside this repo, in the company's "Certs and Logos" folder):

| Certificate | Number | Issued by | Held since | Current issue | Valid until |
|---|---|---|---|---|---|
| bizSAFE Star | E01694 | Workplace Safety and Health Council | — | 30 Apr 2026 | 29 Apr 2029 |
| ISO 9001:2015 (quality) | KWQ-1547 | KW Certification Pte Ltd, SAC accredited (QS-2017-31) | 30 Apr 2014 | 26 Apr 2026 | 29 Apr 2029 |
| ISO 45001:2018 (health and safety) | KWO-1548 | KW Certification Pte Ltd, SAC accredited (OSH-2019-20) | 12 Jun 2015 | 26 Apr 2026 | 29 Apr 2029 |
| EMA Electrical Technician's Licence (the in-house LEW, Teong Hien Sing) | 8/18147 | Energy Market Authority | — | Card issued 16 Aug 2016 | Lifetime of the licence holder |

The LEW licence number is on EMA's public register, so it can be shown on the site. **Never publish his NRIC number**, which is printed on the licence card, and never add a scan of the card to this repo.

Both ISO certificates cover "Provision of Electrical Engineering Service and Installation Works". KW Certification (59 Ubi Avenue 1 #06-04, Singapore 408938) confirms their validity on request.

- **bizSAFE and both ISO certificates expire on 29 April 2029.** (The LEW licence doesn't expire.) Before then, get the renewed certificates and update:
  - the logos (`public/img/bizsafe.*`, `iso9001.*`, `iso45001.*`). The KW logos print the cert numbers inside them, so replace them if the numbers change.
  - the certificate-number captions in the strip
  - the certificate numbers and "since" years in the FAQ answer, the structured data and `llms.txt`
- The site deliberately doesn't show expiry dates or offer certificate copies. This table is the record of when they expire.
- Don't use the SAC mark on its own (the "QS-2017-31 / OSH-2019-20" file). It may only appear combined with KW Certification's mark, as in the ISO logo files.
- Don't recolour or invert the logos. In dark mode they sit on white tiles instead.
- BCA, EMA and SMRT don't give contractors a logo to display, and using their corporate logos would imply endorsement. Their columns use big type headings instead (**L1**, **LEW**, **1998**). Don't swap those for logos.
- The in-house LEW holds an **Electrical Technician's licence** (design up to 150 kVA, operation up to 500 kVA, per EMA's Electrical Installation Licence handbook). Larger installations go to a partner Professional Engineer. Keep the strip, the "Licensed sign-off in-house" point, the EMA licensing service and the licence-renewal FAQ consistent with that, and never claim he signs off every installation.

**Credentials strip layout.** It sits at the bottom of the hero, with a light background in light mode. It has six columns, and each one is a `<div>` with a `<dt>` heading and a `<dd>` description:

```html
<!-- type heading -->
<div><dt><span class="tb"><b>L1</b><small>BCA · ME05</small></span></dt><dd>Description</dd></div>
<!-- logo heading -->
<div><dt class="lg"><img src="img/iso9001.5cd591cb-400.png" alt="…" width="400" height="176" decoding="async" loading="lazy"><small>No. KWQ-1547</small></dt><dd>Description</dd></div>
```

Every heading has a small red caption: what the type heading stands for, the LEW licence number, or the certificate number under each logo. Keep each description to about three lines at desktop width so the columns end level.

### Key figures
The four big numbers sit in "Who we serve", between its intro and the client groups (`<div class="facts">`). Each number is a `<span class="num" data-count="…">`, and the count-up animation reads `data-count`.

| Figure | How it updates | Also appears in |
|---|---|---|
| Years in business | Automatically: a script counts full years since 24 Jul 1973. The `53` in the HTML is only a fallback for crawlers, so bump it each July. | — |
| 200+ development projects | By hand | the projects section intro ("more than 200 projects"), `llms.txt` key facts |
| 300+ clients | By hand | `llms.txt` key facts |
| 3 SMRT rail lines | By hand | the "Lightning protection across three SMRT lines" project card, the "Three SMRT rail lines" timeline entry in About |

### Phone, email, address, hours
These appear in the contact section, the structured data, `llms.txt`, `og-image.png` and the Google Business Profile. Keep them all identical.

### Images
- Export WebP (about 960px wide, quality around 70) and a JPG fallback (about 800px).
- Use a **new file name** for every new or changed image (for example `name.<hash>-960.webp`). Images are cached for a year, so a reused name won't refresh.
- Always include `width`, `height` and descriptive `alt` text.
- **Exception: certification logos** are PNG only, because they need transparency. Export them at twice their display size (about 400px wide for the ISO marks, 240px for bizSAFE). Crop out excess white space, but never alter the artwork.

### Domain
If the site moves to a different address, search and replace `https://www.sengsoon.com.sg/` in `index.html`, `robots.txt`, `sitemap.xml` and `llms.txt`.

### Before each publish
- Check on a phone that nothing scrolls sideways.
- Check the structured data on the live page with the [Schema Markup Validator](https://validator.schema.org/) (no sign-in needed), or Google's [Rich Results Test](https://search.google.com/test/rich-results) while signed in to a Google account.

---

© Seng Soon Electrical Engineering Pte Ltd · UEN 198804759R
