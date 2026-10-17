# Elderfrost Nursery — sunchoke site

A static site selling Jerusalem artichoke tubers and cold-hardy perennial companions
from Elderfrost Nursery — a Zone 5 experimental plant nursery.

**No town or state is named anywhere on the site, by design.** The location descriptor
is `"Zone 5 · Experimental plant nursery"`, held in one field (`location` in
`products.json`) plus the page title in `page_title`. Change it in those two places.

Built for **GitHub Pages**: no build step, no framework, no dependencies.

**Live:** https://elderfrost-farm.github.io/sunchokes/

## The job of this page

Make someone want sunchokes, understand exactly how to grow them, hear about the two
real drawbacks *before* they buy, and then successfully eat them. Checkout happens on
Squarespace — this site links out to it and never touches payment.

## Files

| File | Purpose |
|---|---|
| `index.html` | The whole site: copy, sections, shop grid. |
| `products.json` | **Catalog + site settings.** The only file you normally edit. |
| `assets/style.css` | Core styling. |
| `assets/additions.css` | Hero image, rule-of-5 band, photo band, card alignment. |
| `images/` | Product photos. |
| `.nojekyll` | Serve files as-is (no Jekyll processing). |

## Site settings — `products.json` top block

```json
"site_name":  "Elderfrost Nursery",
"store_url":  "https://www.elderfrost.com",
"shop_url":   "https://www.elderfrost.com/shop",
"location":   "Zone 5 · Experimental plant nursery"
```

`site_name` drives the header wordmark, the footer, and the page title.
`store_url` / `shop_url` fill every outbound link.

## Products

The 12 products, prices and URLs were pulled from your live store
(`www.elderfrost.com/shop?format=json`) and **every product URL was verified to return
HTTP 200**. Categories:

- **Sunchokes** — Jerusalem Artichoke Tubers (8 oz $18 · 1 lb $27 · 3 lb $50)
- **Perennial Vegetables** — Walking Onion Bulbs, Big Top Western Horseradish, Myoga Ginger
- **Cold-Hardy Berries** — Thornless Raspberry, Allegheny Blackberry, Ebony King Blackberry, Swamp Dewberry, Honeyberry Haskap, Black Currant, Elderberry
- **Support Species** — Hybrid Willow

Each entry:

```json
{
  "name": "Jerusalem Artichoke Tubers",
  "subtitle": "8 oz · 1 lb · 3 lb",
  "price": 18.00,
  "from": true,
  "category": "Sunchokes",
  "note": "One or two sentences that sell it.",
  "url": "https://www.elderfrost.com/shop/p/...",
  "image": "images/sunchoke-hand-basket.jpg",
  "badge": "The main event",
  "sold_out": false,
  "visible": true
}
```

- `from: true` renders the price as "from $18". Set `false` for a single fixed price.
- `sold_out: true` greys the card, shows an **Out of stock** badge, and the
  "In stock only" toggle will hide it.
- `badge` is optional — used for "Native", "Only 6 left", etc.
- `image` — leave `""` for a clean letter placeholder tile. 4:5 or 1:1 crop looks best.
- New `category` values automatically create a new filter chip.

### Re-pulling prices from Squarespace

Prices and stock change. To refresh:

```bash
curl -sL "https://www.elderfrost.com/shop?format=json" -o /tmp/shop.json
```

Then read `items[].variants[].price` (cents) and `.qtyInStock`. Update `products.json`
and bump the `sold_out` flags.

## The rule of 5 (in `index.html`, `#rule`)

This is the strongest selling argument on the page and it's your own copy — every pound
planted becomes roughly five pounds in the ground by season's end, twenty-five the year
after. The table is derived directly from that. If you ever change the rule of 5,
change the table.

## Photos in use

| File | Where |
|---|---|
| `sunchoke-hand-basket.jpg` | Hero + the sunchoke product card |
| `sunchoke-flowers.jpg` | Full-width band between Why and the catch |
| `sunchoke-tubers-macro.jpg` | Eat section |
| `sunchoke-branded.jpg` | Favicon (the branded vertical) |

Remaining products use letter placeholders. Drop photos into `images/` and set the
`image` field to add them.

## Editing and deploying

```bash
cd ~/storefront-site
# edit products.json or index.html
git add -A && git commit -m "update" && git push
```

Pages rebuilds in about a minute.

## Local preview

```bash
cd ~/storefront-site
python3 -m http.server 8000
```

Open http://localhost:8000 — not `index.html` directly, because the catalog loads via
`fetch()`, which browsers block on `file://`.

## Still to do

- [ ] Photos for the other 11 products
- [ ] Replace the news-update paragraphs in the product `note` fields with evergreen copy
      (some current notes carry seasonal "Late Winter 2026 Update" framing — those age)
- [ ] Confirm the "single red-skinned strain" line in `#varieties` is how you want it phrased
- [ ] Consider a newsletter signup block — Squarespace's is already on your site
