# PharmacyNextDoor — Hummingbird child theme

A production-oriented **child theme** for PrestaShop 9 / **Hummingbird V2**. It
layers the PharmacyNextDoor design system (teal + lime brand, Montserrat /
Source Sans 3 type, 16px radii, soft mint surfaces) onto the real Hummingbird
templates without forking their markup.

```
hummingbird_pnd/
├── config/
│   └── theme.yml                 # child theme manifest (parent: hummingbird)
├── preview.png                   # theme picker thumbnail
├── assets/
│   └── css/theme.css             # COMPILED drop-in stylesheet (loaded by theme.yml)
└── src/scss/                     # source — mirrors Hummingbird's SCSS architecture
    ├── theme.scss                # entry point / build wiring notes
    ├── abstract/variables/       # _colors.scss, _typography.scss  (theme tokens)
    ├── bootstrap/overrides/variables/
    │   ├── _variables.scss       # global Bootstrap overrides ($primary → teal, fonts, radii)
    │   └── components/           # _buttons, _forms, _card, _index (badges/breadcrumb/…)
    └── prestashop/
        ├── base/                 # _tokens (runtime --pnd-* + --bs-* remap), _fonts, _global
        ├── layout/               # _header, _footer
        ├── components/           # _catalog, _ui, _navigation
        └── pages/                # _index, _category, _product, _cart, _checkout,
                                  #   _customer, _authentication, _content
```

## Two ways it applies

1. **Compiled drop-in (no build):** `assets/css/theme.css` re-maps Hummingbird's
   compiled Bootstrap custom properties (`--bs-primary`, `--bs-body-font-family`,
   `--bs-border-radius`, …) **and** styles real selectors. It loads after the
   parent bundle (`theme.yml` → `priority: 300`), so overrides win without
   `!important`. Works the moment the theme is installed.
2. **Real SCSS build (recommended PR path):** the `src/scss/` tree slots into
   Hummingbird's own pipeline. The Bootstrap variable overrides recompile the
   framework with our tokens (best output), and the `prestashop/` partials add
   the component/page layer. See `theme.scss` for the import order.

## Install

1. Copy `hummingbird_pnd/` into `themes/` of your PrestaShop install.
2. Back-office → **Design → Theme & Logo → Add new theme → Import from FTP**,
   then select & use *PharmacyNextDoor — Hummingbird child*.
3. (Optional) self-host the fonts: replace the `@import` in
   `prestashop/base/_fonts.scss` with `@font-face` and drop files in `assets/`.

## Selector fidelity

Every rule targets **real Hummingbird markup** confirmed against
`PrestaShop/hummingbird@develop` — e.g. `header-bottom__container`,
`header-block__action-btn`, `footer__main-top`, `product-miniature__*`,
`quantity-button`, `account-menu__link`, `cart-grid` / `product-line`,
`checkout-step`, `facet`, `page-link`, `breadcrumb`.

## Coverage matrix

| Area | Templates | Status |
|---|---|---|
| Header / nav / footer | `_partials/header,footer,copyright`, `ps_mainmenu` | ✅ styled |
| Home | `index`, hero slider, trust, rails, `ps_emailsubscription` | ✅ styled |
| Category / listing | `catalog/listing/*`, `facet`/`#search_filters`, sort, pagination | ✅ styled |
| Product miniature | `catalog/_partials/miniatures/*` | ✅ styled |
| Product page | `catalog/product.tpl`, gallery, prices, accordion, reviews | ✅ styled |
| Cart | `cart.tpl`, `cart-detailed`, `product-line`, voucher, summary | ✅ styled |
| Checkout | `checkout/*`, delivery/payment options, summary, confirmation | ✅ styled |
| Customer account | `my-account`, `history`, `order-detail`, `order-follow`, `order-return`, `order-slip`, `addresses`, `address`, `identity`, `discount` | ✅ styled |
| Authentication | `authentication`, `registration`, `password-*`, `guest-*` | ✅ styled |
| CMS / content | `cms/page`, contact, stores, sitemap, manufacturers/suppliers | ✅ styled |
| Errors | `errors/*`, `page-not-found`, maintenance | ✅ styled |
| Forms / tables / alerts | `form-fields`, `grid-table`, `notifications`, `breadcrumb` | ✅ styled |

**Not in scope (core, not theme):** transactional **email templates**
(`mails/`) live in PrestaShop core / modules and are styled separately;
business logic, controllers and Smarty data are untouched per Hummingbird's
theme-boundary rules.

## Design tokens

| Token | Value |
|---|---|
| Primary (brand) | `#15665c` teal-green |
| Brand dark / hover | `#0f5249` · `#0c3f39` |
| Accent | `#8dc63f` lime |
| Sale / danger | `#d8453b` |
| Surfaces | `#eef5f3` mint · `#f6faf9` |
| Ink / body / muted | `#16211e` · `#36433f` · `#6a7672` |
| Lines | `#e6ecea` |
| Radius | `16px` (sm `8px`, pill) |
| Headings | Montserrat 600/700 |
| Body | Source Sans 3 400–700 |

## Rebuild the compiled CSS

`assets/css/theme.css` is generated from the `prestashop/` partials (token layer
first, then base → layout → components → pages). Recompile with the parent
Hummingbird build, or re-concatenate the partials in that order.
