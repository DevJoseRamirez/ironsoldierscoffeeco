# Dawn Rebuild Plan

**Approach:** fresh Dawn base → brand tokens + voice → rebuild custom features the Dawn way → new showcase sections.
Companion to [theme-custom-inventory.md](theme-custom-inventory.md).

---

## 1. Brand tokens (starting point, pulled from current theme)

These are what the live store uses today. Confirm or replace with the brand guide before locking them in.

### Colors

| Token | Value | Where it shows today |
|---|---|---|
| `--color-primary` (olive / military green) | `#353b25` | Footer background, checkout buttons/accents |
| `--color-text` | `#111111` / `#333333` | Body text |
| `--color-header-text` | `#343434` | Header |
| `--color-bg` | `#ffffff` | Body, header |
| `--color-on-primary` | `#ffffff` | Footer text |
| `--color-border` | `#e5e5e5` | Borders |
| `--color-accent-teal` | `#7fc9c4` | Cart count badge, mobile theme-color (likely demo leftover — confirm) |
| **Trading Company** `--tc-bg` | `#FAF3E6` | Cream page background |
| **Trading Company** `--tc-footer` | `#352812` | Dark brown footer |
| **Doc Sims Brew** button | `#553472` / hover `#451271` | Product add-to-cart |
| **Dany's D-Day Blend** button | `#211c6b` / hover `#433e83` | Product add-to-cart |

### Typography
- **Headings / UI:** Sofia Pro (self-hosted from vendor CDN today — upload own licensed files)
- **Fallback / secondary:** Poppins (theme setting)
- **Buttons:** Karla
- **Checkout:** Helvetica Neue

> Pick 1–2 families for Dawn. Dawn's font picker only offers Shopify-hosted fonts; Sofia Pro would need custom `@font-face` in a snippet.

### Logos
- Main: `Iron_Soldier_Coffee_Company.png` (max width 300px)
- Trading Company: `Iron_Soldier_Trading_Company-1.png`

### Brand voice
Not in the theme — add from brand guide:
- [ ] Tone / voice notes
- [ ] Button & CTA wording (today: "Add to Cart", "Shop now", "Get in Touch")
- [ ] Veteran / military story messaging

### Dawn color schemes to set up
| Scheme | Background | Text | Button | Use |
|---|---|---|---|---|
| scheme-1 (default) | `#ffffff` | `#111111` | `#353b25` | Most pages |
| scheme-2 (inverse) | `#353b25` | `#ffffff` | `#ffffff` | Footer, feature bands |
| scheme-3 (Trading Co.) | `#FAF3E6` | `#352812` | `#352812` | Trading Company pages |
| scheme-4 (Trading Co. dark) | `#352812` | `#FAF3E6` | `#FAF3E6` | Trading Company footer |

---

## 2. Rebuild old customs the Dawn way

| Old (Cufee) | Dawn approach |
|---|---|
| 13 near-identical product templates | 1 product template + `custom.story_page` page metafield, rendered in a collapsible tab / story block |
| `grind-types` tab | Collapsible-tab block pointing at the `grind-types` page (shared) |
| `custom.product_description_snippet` | Text block with dynamic source → metafield |
| Hardcoded per-product button colors | `custom.button_color` color metafield, applied with a small style block (no handles in code) |
| Trading Company `handle ==` checks in 7 files | Color scheme 3/4 + separate header/footer **section groups** or a `theme.trading` alternate layout; menu + logo picked in section settings |
| Trading Co. product styling only via collection | Use product metafield / tag (e.g. `custom.brand = trading-company`) so direct links work too |
| "From $X" pricing on cards | Small edit to `card-product.liquid` (only when variants have different prices) |
| Square product images | Dawn setting: Product card → Image ratio = Square |
| Collection card image fallback | Dawn already falls back to first product image |
| Clickable banner images | Build into new banner sections |
| Wishlist removed | Not in Dawn — nothing to do |
| Judge.me snippet widget | Judge.me app block on product template + star badge app block on cards |
| Bold Subscriptions snippet in `theme.liquid` | Bold app block / Shopify subscriptions app block |
| Clarity + Google verification in `theme.liquid` | Add back to `layout/theme.liquid` `<head>` (or Clarity via app / Customer Events pixel) |

---

## 3. Sections to build / port

### Port existing CWC sections (clean up for Dawn color schemes)
- [ ] CWC Three Image Banner (desktop/mobile images + link)
- [ ] CWC Two Image Banner
- [ ] CWC Network Cards (Allied Network)
- [ ] Partner CTA (from `ai_gen_block_0ae02b1`)

### New showcase sections (ideas — confirm scope)
- [ ] Hero / brand story (veteran mission)
- [ ] Vehicle story feature (image + story + linked coffee)
- [ ] Coffee finder: filter by roast / type using `custom.coffee_roast_select`, `custom.coffee_type_select`
- [ ] Grind types explainer
- [ ] Reviews / social proof band (Judge.me)
- [ ] Trading Company landing hero
- [ ] Subscription promo (Bold)

### Pages to rebuild
- [ ] Home
- [ ] Trading Company (page + collection + product)
- [ ] Allied Network
- [ ] About (+ US market override)
- [ ] Contact, FAQs
- [ ] Ebook, QR code signup, Peanut Festival
- [ ] Judge.me all reviews

---

## 4. Store-side setup (admin, not theme)

- [ ] Create metafield definitions: `custom.story_page` (page ref), `custom.button_color` (color), `custom.brand` (or use tags)
- [ ] Fill `custom.story_page` per coffee product (map from old `add-tab_2` values in the inventory table)
- [ ] Confirm menus: main, mobile, `trading-company`
- [ ] Re-enable app embeds after publish: Judge.me, Bold, Printful, Simple Google Shopping
- [ ] Reassign product/page/collection templates once the new theme is live
- [ ] Upload licensed Sofia Pro files (if kept)

---

## 5. Order of work

1. Pull fresh Dawn into a new folder / branch
2. Set color schemes, fonts, logo, buttons (section 1)
3. Product template + metafields (section 2) — biggest impact
4. Port CWC sections
5. Trading Company look
6. Build pages + new showcase sections
7. Apps, tracking, QA, reassign templates, publish
