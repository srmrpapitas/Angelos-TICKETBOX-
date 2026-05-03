# Ticket Box Villamar · Angelos agent

Premium Tenerife excursions and watersports website. Single-page, fully static, mobile-first, with EN/FR/ES support, satellite map, and one-tap WhatsApp booking.

🌐 **Live site:** _to be deployed on Cloudflare Pages_
📞 **Booking:** [+34 635 99 96 35](https://wa.me/34635999635) (WhatsApp)
👤 **Owner / agent:** Angelo

---

## ✨ What's inside

- **14 curated excursions** with full details, prices and pickup points
- **Multilingual** — English, French, Spanish (toggle in the top nav)
- **Live booking modal** — pick option, date and party size, confirm via WhatsApp with a pre-filled message
- **Satellite map of Tenerife** (Esri imagery) with all departure points
- **Local image assets** — fast-loading, no third-party CDN
- **Pure HTML/CSS/JS** — no build step, no framework, deploys anywhere
- **Self-contained** — only one external dependency (Leaflet for the map, loaded from CDN)

## 📁 File structure

```
.
├── index.html        # The entire website (HTML + CSS + JS)
├── images/           # All 13 excursion photos
│   ├── whale-watching.jpeg
│   ├── private-charter.jpeg
│   ├── deep-fishing.jpeg
│   ├── anaga.jpeg
│   ├── masca.jpeg
│   ├── teide.jpeg
│   ├── buggy.jpeg
│   ├── jetski.jpeg
│   ├── kayak.webp
│   ├── parasailing.jpg
│   ├── paragliding.jpeg
│   ├── self-drive-boat.webp
│   └── dinner-show.jpeg
├── README.md
└── .gitignore
```

---

## 🚀 Deploying to Cloudflare Pages

### Step 1 — Push to GitHub

```bash
# In the project folder
git init
git add .
git commit -m "Initial site"

# Create a new GitHub repo (private or public), then:
git remote add origin https://github.com/<your-username>/angelos-ticket-box.git
git branch -M main
git push -u origin main
```

### Step 2 — Connect Cloudflare Pages

1. Go to [dash.cloudflare.com](https://dash.cloudflare.com) → **Workers & Pages** → **Create** → **Pages** → **Connect to Git**
2. Authorize GitHub and select the repo `angelos-ticket-box`
3. Build settings:
   - **Framework preset:** _None_
   - **Build command:** _(leave empty)_
   - **Build output directory:** `/` (project root)
4. Click **Save and Deploy**

Cloudflare assigns you a `*.pages.dev` URL within ~30 seconds.

### Step 3 — Custom domain (optional)

In the Pages project → **Custom domains** → **Set up a custom domain** → enter `ticketboxvillamar.com` (or any domain you own). Cloudflare automatically provisions SSL.

### Step 4 — Updating the site

Any push to `main` redeploys automatically. No build step, no waiting.

```bash
git add .
git commit -m "Update prices for whale watching"
git push
```

---

## ✏️ Editing content

All editable content lives in `index.html`. Here's where to find what:

| What | Where (in `index.html`) |
|---|---|
| Excursion list, prices, options | `const EXCURSIONS = [...]` (around line 1325) |
| Translations (EN / FR / ES) | `const I18N = { en: {...}, fr: {...}, es: {...} }` |
| Phone / WhatsApp number | Search for `34635999635` (4 spots) |
| Map default view | Search for `setView([28.2916, -16.6291], 10)` |
| Hero headline | Search for `data-i18n="hero.title2"` |

### Editing an excursion

Each excursion is an object like this:

```js
{
  id: 1,
  cat: 'sea',
  title:    { en: "...", fr: "...", es: "..." },
  desc:     { en: "...", fr: "...", es: "..." },
  img:      "images/whale-watching.jpeg",
  duration: { en: "...", fr: "...", es: "..." },
  badge:    { en: "...", fr: "...", es: "..." },   // optional
  startPoint: { name: "...", lat: 28.0834, lng: -16.7402 },
  options: [
    { name: { en, fr, es }, desc: { en, fr, es }, price: 30 },
    // mark optional add-ons (don't count for "from" price):
    { name: ..., desc: ..., price: 15, addon: true },
    // mark "request a quote" options:
    { name: ..., desc: ..., price: 0, quote: true }
  ],
  videoQuery: "search query for the YouTube embed in the modal"
}
```

### Adding a new language

Copy the `en: { ... }` block inside `I18N`, translate every value, and add a button to the `.lang-switch` in the nav (search for `data-lang="en"`).

### Replacing a photo

1. Drop the new image into `images/`
2. Update the `img:` path inside the matching excursion in `EXCURSIONS`
3. Commit + push

Recommended size: ~1200×900 px, JPG or WebP, under 300 KB.

---

## 🗺️ Map

Uses [Leaflet](https://leafletjs.com/) with [Esri World Imagery](https://www.arcgis.com/home/item.html?id=10df2279f9684e4a9f6a7f08febac2a9) tiles. Pins are auto-generated from each excursion's `startPoint`. Multiple tours sharing the same coordinates are grouped into one pin with a list popup.

To add a new pickup point, just edit the `lat` and `lng` of any excursion in `EXCURSIONS` — the map will update automatically.

---

## 📱 WhatsApp booking flow

When a visitor hits "Confirm via WhatsApp", the site builds a message in their selected language (EN/FR/ES) with:

- Tour name
- Selected option + price (or "Quote" if it's a quote-only option)
- Date
- Number of people
- Departure point
- Total

…and opens `https://wa.me/34635999635?text=<encoded message>`. Angelo gets a WhatsApp with everything he needs to confirm.

---

## 🛠️ Local preview

You don't need any tooling. Just open `index.html` in a browser:

```bash
# Either:
open index.html

# Or run a tiny local server (recommended for the map):
python3 -m http.server 8000
# then visit http://localhost:8000
```

---

## 📄 License

Private project. All excursion content, photos and copy © Ticket Box Villamar / Angelo.
