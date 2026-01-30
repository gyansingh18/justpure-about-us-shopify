# Product card: Bestsellers layout (Featured collection only)

This adds a **conditional layout** for the product card: only when the card is rendered inside your **"Our Bestselling Products" / Featured collection** section, the order is: **Image → Add to Cart → Title → Price**. Everywhere else (collection pages, search, other sections) the card stays **Image → Title → Price → Add to Cart**.

## What’s included

- **`snippets/card-product.liquid`** – Product card with `layout_bestsellers` conditional and scoped CSS.
- **`snippets/card-product-form.liquid`** – Single add-to-cart form (variant selection + button); reused, no duplication.
- **`snippets/card-product-price.liquid`** – Price block.
- **`sections/featured-collection.liquid`** (optional) – Featured collection section that passes `layout_bestsellers` and disables lazy load for above-the-fold images.

## Setup

### 1. Upload snippets

Upload to your theme:

- `snippets/card-product.liquid`
- `snippets/card-product-form.liquid`
- `snippets/card-product-price.liquid`

### 2. Use the bestsellers layout only in the Featured/Bestsellers section

**Option A – You already have a Featured collection section (e.g. Dawn `featured-collection.liquid`)**

1. Open **Sections → featured-collection.liquid** (or the section that powers “Our Bestselling Products”).
2. Add a setting so you can turn the bestsellers layout on only for this section:

In the **schema** (at the bottom), add inside the `"settings"` array:

```json
{
  "type": "checkbox",
  "id": "use_bestsellers_layout",
  "label": "Use bestsellers layout (image, then Add to Cart, then title & price)",
  "default": false
}
```

3. Where the section renders the product card, pass `layout_bestsellers` and control `lazy_load` for the first row.

Find the loop that renders the card (e.g. `for product in section.settings.collection.products`). It might look like:

```liquid
{% for product in section.settings.collection.products limit: section.settings.products_to_show %}
  {% assign lazy_load = false %}
  {% if forloop.index > max_columns_to_show %}
    {% assign lazy_load = true %}
  {% endif %}
  {% render 'card-product',
    card_product: product,
    section_id: section.id,
    lazy_load: lazy_load,
    ...
  %}
{% endfor %}
```

Change it so that when “Use bestsellers layout” is on, you pass `layout_bestsellers: true` and keep the first row non–lazy-loaded:

```liquid
{% assign use_bestsellers = section.settings.use_bestsellers_layout | default: false %}
{% for product in section.settings.collection.products limit: section.settings.products_to_show %}
  {% assign lazy_load = true %}
  {% if forloop.index <= max_columns_to_show %}
    {% assign lazy_load = false %}
  {% endif %}
  {% if use_bestsellers and forloop.index <= max_columns_to_show %}
    {% assign lazy_load = false %}
  {% endif %}
  {% render 'card-product',
    card_product: product,
    section_id: section.id,
    lazy_load: lazy_load,
    layout_bestsellers: use_bestsellers,
    skip_styles: skip_card_product_styles,
    media_aspect_ratio: section.settings.image_ratio,
    show_vendor: section.settings.show_vendor
  %}
  {% assign skip_card_product_styles = true %}
{% endfor %}
```

(Adjust variable names like `max_columns_to_show` and `skip_card_product_styles` to match your theme. The important parts are `layout_bestsellers: use_bestsellers` and `lazy_load: false` for the first row when bestsellers layout is on.)

4. If your theme currently uses a **different** snippet name for the card (e.g. a custom `card-product-custom.liquid`), you have two choices:
   - Use this new `card-product` only in this section (as in the `render` above), and leave other sections using the old snippet, **or**
   - Merge the conditional layout (and the form/price sub-snippets) into your existing card snippet and keep calling that snippet everywhere; in the Featured/Bestsellers section, pass `layout_bestsellers: true` and `lazy_load: false` for the first row as above.

**Option B – Use the provided featured-collection section**

- Add **`sections/featured-collection.liquid`** from this repo to your theme (or replace your existing one if you’re okay with that).
- In the theme editor, open the **Featured collection** block that you use for “Our Bestselling Products”.
- Enable **“Use bestsellers layout”**.
- Other Featured collection sections that do **not** use this option will keep the default card order.

### 3. AJAX cart and variant selection

The add-to-cart form uses the standard Shopify `form 'product', product` with `data-type: 'add-to-cart-form'`. If your theme’s global JS submits forms with that attribute via AJAX, the same behavior will apply to this card. Variant selection is preserved via the hidden/select `id` field in `card-product-form.liquid`.

## Constraints (as requested)

- Layout change **only** when the product card is rendered inside the Bestsellers/Featured collection section (and only when you enable the option above).
- Other collection pages, product pages, and other sections are unchanged.
- Single add-to-cart implementation (form in `card-product-form.liquid`), no duplicated logic.
- Scoped CSS; no layout shift (aspect ratio and flex order are set).
- Above-the-fold product images in this section are not lazy-loaded when bestsellers layout is on (`lazy_load: false` for the first row).
