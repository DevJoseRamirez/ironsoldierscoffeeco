# Theme Migration — Custom Code Inventory

**Store:** Iron Soldiers Coffee Co.
**Current theme:** Cufee v1.0.0 (Shopilaunch)
**Reviewed:** 2026-11-01

**Method:** compared the first git commit (`f79d7e9`, 2025-05-13) against `HEAD`, ignoring whitespace-only changes, and mapped every JSON template to its sections. Anything added before that first commit (Clarity, Bold, Judge.me) was identified from the code itself.

> Template *assignments* (which products/pages actually use which template) were not verified. Check them in the Shopify admin before dropping any template.

---

## 1. Custom-built files (move all of these)

| File | What it does | Used on |
|---|---|---|
| `sections/CWC_three_image_banner.liquid` + `assets/CWC_three_image_banner.css` | 3 linked banners, separate desktop/mobile images | Home (top section) |
| `sections/CWC_two_image_banner.liquid` + `assets/CWC_two_image_banner.css` | 2 linked banners, desktop/mobile images | Home (currently disabled) |
| `sections/CWC_network_cards.liquid` + `assets/CWC_network_cards.css` | Partner cards: image, title, text, 2 buttons | Allied Network page (5 instances) |
| `blocks/ai_gen_block_0ae02b1.liquid` | "Want to Partner with Iron Soldiers?" CTA | Allied Network page |
| `blocks/ai_gen_block_3397fdc.liquid`, `ai_gen_block_9543fd2.liquid`, `ai_gen_block_97aee7c.liquid` | AI-generated content / image blocks | Not placed anywhere (product-template `_blocks` sections are empty) |
| `snippets/CWC_button-styles.liquid` | Per-product add-to-cart colors: `doc-sims-brew` (purple `#553472`), `dany-s-d-day-blend` (navy `#211c6b`) | Product page |
| `snippets/judgeme-review-widget.liquid` | Judge.me review widget below product tabs | Product page |
| `assets/CWC_flagship.css` | Not referenced anywhere — dead | — |

The CWC sections are self-contained and should port to an Online Store 2.0 theme with little change.

- [ ] CWC_three_image_banner
- [ ] CWC_two_image_banner
- [ ] CWC_network_cards
- [ ] ai_gen_block_0ae02b1 (partner CTA)
- [ ] Per-product button colors
- [ ] Judge.me product widget placement

---

## 2. "Trading Company" sub-brand (largest rebuild item)

A second look is hardcoded via `page.handle == 'trading-company'` and `collection.template_suffix == 'trading-company'` checks across seven files:

- **Background / footer colors** — cream `#FAF3E6`, brown footer `#352812`
  - `layout/theme.liquid:128`
  - `snippets/footer-v4.liquid:164`
  - `snippets/collection-infinity-scroll.liquid:1`
  - `snippets/product-extended.liquid:2`
- **Separate logo** — hardcoded CDN URL `Iron_Soldier_Trading_Company-1.png`; logo links to `/pages/trading-company`
  - `snippets/header-v1.liquid:185` (desktop)
  - `snippets/menu_mobile.liquid:19` (mobile)
- **Separate navigation** — uses the `trading-company` menu
  - `snippets/eveland-menu.liquid:7` (desktop)
  - `snippets/menu_mobile.liquid:89` (mobile)
- **Templates:** `page.trading-company.json`, `collection.trading-company.json`, `product.trading-company.json`

Notes:
- `assets/engo-customizes.css.liquid:33530` has a leftover red-background rule. Liquid in a CSS asset can't see `page`, so it never runs. Don't port.
- Product pages only get Trading Company styling when reached through that collection (`collection` is empty on a direct product URL).

**Recommendation:** in the new theme, implement via its own color scheme + header/footer section groups (or a theme setting / metafield), not hardcoded handles.

- [ ] Trading Company colors
- [ ] Trading Company logo
- [ ] Trading Company menu
- [ ] Trading Company page / collection / product templates

---

## 3. Modifications to stock Cufee files

| File | Change |
|---|---|
| `sections/product-tabs.liquid` | Two extra tabs pulling page content (`add-tab_1` / `add-tab_2`). Coffee templates use `grind-types` + a vehicle story page. Description links styled blue + underlined. |
| `snippets/product-extended.liquid` | Shows `custom.product_description_snippet` metafield instead of truncated description. Wishlist disabled. |
| `snippets/product-details-large-image.liquid:270` | Hardcoded purple add-to-cart button for `doc-sims-brew`. |
| `snippets/product-item-v1.liquid` | Prices show "From $X". Judge.me star badge removed. Wishlist hidden. |
| `snippets/collection-infinity-scroll.liquid` | Forces square (1:1) product images. |
| `sections/section-collection-v2.liquid` | If a block has no image, falls back to first product image of the linked collection. |
| `sections/section-banner-v8.liquid` | Whole image is clickable, not just the button. |
| `sections/section-banner-v2.liquid` | Subtitle changed to rich text. |
| `section-info-v1`, `section-product-v2`, `section-banner-v8`, `section-collection-v2` | Top/bottom spacing switched from margin to padding. |
| `assets/engo-customizes.css.liquid` | Text colors set to `inherit`, breadcrumb padding, footer link hover uses opacity. |
| `sections/product-hover.liquid` | Wishlist hidden. |

- [ ] Extra product tabs (grind types + vehicle story)
- [ ] Description snippet metafield
- [ ] "From $X" pricing on cards
- [ ] Square product images
- [ ] Collection image fallback
- [ ] Wishlist removed everywhere

---

## 4. Templates

### Product templates (13)
armored-car, doc-sims-brew, dodge-ambulance, g503-jeep, gmc-troop-truck, halftrack, m29-weasel, sherman-tank, merchandise, rubs-n-sauces, trading-company, default.

They're nearly identical — the only real difference is which story page is in `add-tab_2`:

| Template | add-tab_1 | add-tab_2 | Reviews tab |
|---|---|---|---|
| product (default) | grind-types | about-us | off |
| armored-car | grind-types | about-us | off |
| doc-sims-brew | grind-types | about-us | off |
| dodge-ambulance | grind-types | the-lifeline-of-the-front-lines-the-dodge-wc-54-ambulance | off |
| g503-jeep | grind-types | g503-jeep | off |
| gmc-troop-truck | grind-types | the-gmc-cckw | off |
| halftrack | grind-types | the-m3-halftrack | off |
| m29-weasel | grind-types | the-m29-weasel | off |
| sherman-tank | grind-types | sherman-tank | off |
| merchandise | grind-types | — | on |
| rubs-n-sauces | — | — | on |
| trading-company | — | — | off |

**Recommendation:** one product template + a page-reference metafield (e.g. `custom.story_page`) instead of 13 templates.

### Pages worth rebuilding
- `index.json` — CWC three-image banner, info, video, banners, collections, product tabs, Judge.me, blog
- `page.trading-company.json`
- `page.allied-network.json`
- `page.ebook.json`
- `page.qr-code-signup.json`
- `page.peanut-festival.json`
- `page.aboutus.json` + `page.aboutus.context.us.json` (US-market override)
- `page.FAQs.json`, `page.contactus.json`
- `page.judgeme_all_reviews.liquid`

### Probably junk (verify assignment first)
- `page.the-armored-car`, `page.the-gmc-cckw`, `page.the-gmc-cckw-2` — copies of the contact page
- `page.g503-jeep` — empty
- Demo leftovers: `page.lookbook1/2`, `page.product-hover`, `page.aboutus-v2/v3/v4`, `page.contact-2/3`, `page.contact-us-v2/v3`, `page.instagram`, `page.wishlist`, `collection.24/48/coffee/tea/list/alternate/merch/default-merchandise`, `collection.sub-collection1–4`

---

## 5. Apps, tracking & data

### Hardcoded in `layout/theme.liquid` (re-add manually)
- [ ] Microsoft Clarity — ID `i47f7dhyjo`
- [ ] Google site verification meta tag (`q6a8D5AIYMemNiCG8Vjg4L0JHvlJhQED89WagyXJAvg`)
- [ ] Bold Subscriptions snippet (`snippets/bold-subscription-widget.liquid`)

### App blocks / embeds
- [ ] Bold Subscriptions — block inside product-template
- [ ] Judge.me — home + trading-company pages (product-page app blocks are disabled; widget comes from the snippet instead), all-reviews page
- [ ] Printful — reads `printful.*` metafields
- [ ] Simple Google Shopping (`mm-google-shopping`) — app embed

### Metafields to support
- [ ] `custom.product_description_snippet` (rich text) — shown on product page
- [ ] `custom.coffee_type_select` — not used by the theme (filters only?)
- [ ] `custom.coffee_roast_select` — not used by the theme (filters only?)

### Navigation
- [ ] Main menu (desktop)
- [ ] Mobile menu
- [ ] `trading-company` menu

---

## 6. Don't carry over (bugs / fragile code)

- Stray debug text likely visible on the live site:
  - `Z` at `snippets/product-item-v1.liquid:170` (product cards)
  - `dddd` at `sections/product-hover.liquid:1152`
- `sections/section-banner-v8.liquid` — `<style>` block was scrambled by a code formatter; per-section title/button color settings no longer work.
- Sofia Pro fonts load from the theme vendor's demo store CDN (`assets/engo-customizes.css.liquid:17-59`). Will break when they remove the files — upload your own copies if keeping the font.
- Trading Company logo is a hardcoded CDN URL — use a theme setting in the new theme.
