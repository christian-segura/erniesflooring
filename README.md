# Ernie's Floor Covering — Website

A one-page marketing site for Ernie's Floor Covering, a licensed (CA Lic. #1129902) commercial and residential flooring installer serving San Diego and the South Bay.

Plain static HTML: no build step, no framework, no external fonts or scripts. CSS, JS and the icon sprite are all inline in `index.html`.

## Project structure

```
index.html     The whole site (markup, inline CSS, inline JS, JSON-LD structured data)
images/        Job photos (WebP) + og-share.jpg (1200×630 social preview image)
robots.txt     Allows all crawlers, points to the sitemap
sitemap.xml    Single-URL sitemap
```

## Local preview

Serve the folder over HTTP rather than opening the file directly (the form will not submit from `file://`):

```sh
python3 -m http.server 8000
# then open http://localhost:8000
```

## Page sections

Hero + lead form · New customer offer · Services · Commercial · Recent work · Process · Referral program · Service area + FAQ · Final CTA · Footer · Mobile quick-contact bar

## Lead form

The estimate form (`#lead`) posts to [FormSubmit](https://formsubmit.co), which emails each lead to the address in the form's `action`.

- Submits via AJAX to `formsubmit.co/ajax/…` and only shows the "Got it, thanks!" message when FormSubmit confirms `success: true`. Otherwise it falls back to a regular form post.
- Client-side validation: name and flooring type required, phone ≥ 10 digits, ZIP exactly 5 digits.
- Spam protection: `_honey` honeypot field; FormSubmit captcha is disabled (`_captcha=false`).
- Lead attribution: the hidden `source` field records UTM parameters / `gclid` or the referrer.
- Fires a `generate_lead` event if Google Analytics (`gtag`) is installed.

## Launch checklist / TODOs

### Must confirm with Ernesto before going live
- [ ] **New customer offer** ($100 Visa gift card on completed install). It was only proposed in a text thread, and Ernesto hasn't agreed to it yet. Remove the `.offer` block from `index.html` if he declines.
- [ ] **Referral program terms** ($100 to the referrer + $100 to the new customer, issued after install is completed and paid in full). Also referenced in the form's "Referred by someone?" field label.

### Domain
- [ ] Buy the domain. Everything currently assumes `https://www.erniesfloorcovering.com/`.
- [ ] If the domain differs, find-and-replace it in `index.html` (canonical, Open Graph / Twitter tags, JSON-LD), `robots.txt` and `sitemap.xml`.

### Form setup (FormSubmit)
- [ ] Submit one test lead from the live site. FormSubmit sends an activation email to the form's address; Ernesto must click it, or no leads will be delivered.
- [ ] After activation, replace the email in the form `action` with the random alias FormSubmit provides, so the address isn't exposed to scrapers.

### SEO / local
- [ ] Set up a Google Business Profile with exactly the same name, address and phone as the site.
- [ ] Get business hours, map coordinates (geo) and social profile links from Ernesto and add them to the JSON-LD (`openingHoursSpecification`, `geo`, `sameAs`).
- [ ] Verify the site in Google Search Console and submit `sitemap.xml`.
- [ ] Ask past customers for Google reviews.
- [ ] After launch, check link previews with Facebook's Sharing Debugger and LinkedIn's Post Inspector.

### Optional
- [ ] Add a 180×180 PNG `apple-touch-icon` (the favicon is an inline SVG, which iOS doesn't use for home-screen icons).
- [ ] Add Google Analytics if wanted; the form already fires a `generate_lead` event.

## Deployment

Any static host works (GitHub Pages, Netlify, Cloudflare Pages). Note that free GitHub Pages requires a **public** repository; Netlify and Cloudflare Pages can deploy from a private one.
