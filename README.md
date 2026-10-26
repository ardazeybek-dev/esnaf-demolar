# esnaf-demolar

Working demo websites for small Turkish businesses (*esnaf*), plus a showcase page with packages and prices. Every site is mobile-first, has no build step and loads in about a second on a phone. All business names, addresses and phone numbers are fictional.

**Live:** https://ardazeybek-dev.github.io/esnaf-demolar/

| Demo | What it shows | Link |
|---|---|---|
| Café / restaurant: *Pervane* | QR menu loaded from one JSON file, categories, allergen icons, TR/EN switch, table number from the QR link | [kafe/](https://ardazeybek-dev.github.io/esnaf-demolar/kafe/?masa=4) · [QR card generator](https://ardazeybek-dev.github.io/esnaf-demolar/kafe/qr.html) |
| Barber: *Makas & Ustura* | Price list with durations, booking picker that writes a ready WhatsApp message, live open/closed status, static map | [kuafor/](https://ardazeybek-dev.github.io/esnaf-demolar/kuafor/) |
| Dental clinic: *Mavikum* | Treatments, process, team, FAQ, consent-aware request form (demo, nothing is sent) | [klinik/](https://ardazeybek-dev.github.io/esnaf-demolar/klinik/) |
| Car wash: *Köpük Garaj* | Packages whose prices change with vehicle type, extras table, before/after slider, WhatsApp | [oto/](https://ardazeybek-dev.github.io/esnaf-demolar/oto/) |

Each sector has its own art direction: an editorial serif menu for the café, dark brass for the barber, a calm teal layout for the clinic and an industrial black-and-yellow look for the car wash.

## Run locally

```bash
cd docs
python -m http.server 8000
# open http://localhost:8000/
```

Plain HTML, CSS and JavaScript. No dependencies, no build, no environment variables. The only external script is `qrcode-generator@1.4.4` from cdnjs, pinned with SRI, and only the QR page uses it. Fonts are self-hosted woff2 files (latin + latin-ext for Turkish characters).

## Structure

```
docs/                 GitHub Pages root
  index.html          showcase: demo cards, packages, FAQ
  kafe/index.html     QR menu (renders menu.json)
  kafe/menu.json      all menu content: categories, items, prices, allergens, TR/EN text
  kafe/qr.html        printable table cards with generated QR codes and PNG download
  kuafor/index.html   barber
  klinik/index.html   dental clinic
  oto/index.html      car wash
  assets/fonts/       self-hosted variable fonts
  assets/shots/       thumbnails used on the showcase page
```

## Adapting a demo for a customer

- **Café menu:** edit only `kafe/menu.json`. Items take `name`, `desc`, `price`, `allergens` (keys from the `allergens` block) and an optional `"tag": "signature"`. The printed QR codes point at the page, not at the menu content, so the cards never need reprinting when the menu changes.
- **Phone / WhatsApp:** search for `905550000000` and `0555 000 00 00` and replace them.
- **Opening hours:** the barber's `HOURS` array drives the hours table, the open/closed badge and the bookable time slots.
- **Car wash prices:** `data-base` on each package and the `EXTRAS` array. The SUV and van multipliers are the `data-k` values on the vehicle buttons.
- Remove the "Bu bir demo tasarımdır" line from the footer before handing a site over.

## Pricing rationale

The showcase lists three one-off packages: **Başlangıç ₺1.500**, **Standart ₺3.500** and **Pro ₺6.500**, plus optional upkeep at ₺750 a year.

- **Hosting is free, so there is no monthly fee.** Static sites run on GitHub Pages or Cloudflare Pages. For a shop owner, "no rent" is the strongest argument against website-builder subscriptions. The domain is the only recurring cost. Standart and Pro include it for the first year. After that it is billed at the registrar's price with no markup, and Başlangıç ships on a free `*.pages.dev` address.
- **The anchor is a real sale.** A previous job closed at ₺4.000 after the customer saw a working demo first. Standart sits just under that proven figure. Pro covers the extra work: more pages, a second language, and prices editable from Google Sheets.
- **The Bionluk gigs stay as the entry point.** The existing listings (one-page site ₺350, custom site ₺400+) are well under the packages here on purpose. With a new profile and no reviews, the low gig gets the first orders and ratings: a single page, the customer supplies all content, a free subdomain, one revision. After 3 to 5 reviews, raise the gig toward Başlangıç. Use the showcase prices when talking to customers outside Bionluk.
- **Upkeep is recurring income.** Menus, prices and hours change every season. ₺750 a year turns one-off customers into a small repeat base without promising unlimited work.

## Pitfalls

- Menu shows "yükleniyor" forever when opened by double-clicking the file → `fetch('menu.json')` is blocked on `file://` → always serve `docs/` over HTTP.
- Parts of an SVG drawing drift apart when built from a reused `<symbol>` → `<use>` without `width`/`height` scales the symbol to the whole viewport → give every `<use>` explicit `width` and `height`.
- In uppercase display type the dots of `İ` and the cedillas of `Ş` hit the neighbouring line → line-height below ~0.9 is too tight for Turkish → keep display line-height ≥ 0.92.
- The page scrolls sideways on phones → a horizontally scrolling chip nav inside a `1fr` grid column forces the column wider → use `minmax(0,1fr)` / `min-width:0` on grid children.
- Clinic sites in Turkey must not advertise prices, patient testimonials or before/after photos (health advertising regulation) → keep the clinic template informational and use before/after galleries only in non-health sectors.
