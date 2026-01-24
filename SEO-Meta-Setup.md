# JustPure SEO Meta – Title, Meta Description, Open Graph

## Overview

The `snippets/seo-meta.liquid` snippet provides optimized `<title>`, `<meta name="description">`, and Open Graph tags (`og:title`, `og:description`, `og:url`, `og:image`, `og:type`) for:

- **Home** – JustPure India, A2 Bilona Ghee, Pure Indian Foods
- **Product pages** – Product Name | Just Pure India; benefits + traditional preparation + purity
- **About page** – About Just Pure India | Healthy Choice Wealthy Life (matches `/pages/our-story` or `/pages/about-us`)

All other pages use Shopify’s default `page_title` and `page_description`.

---

## Installation

### 1. Add the snippet

1. In **Shopify Admin** → **Online Store** → **Themes** → **Edit code**.
2. Under **Snippets**, click **Add a new snippet**.
3. Name it `seo-meta`.
4. Paste the contents of `snippets/seo-meta.liquid` and save.

### 2. Update the layout

1. Open **Layout** → `theme.liquid`.
2. In `<head>`, find the existing `<title>...</title>` and `<meta name="description" ...>` (if present).
3. **Replace** them with:
   ```liquid
   {% render 'seo-meta' %}
   ```
4. If your theme uses a **social meta** or **Open Graph** snippet (e.g. `snippets/social-meta-tags.liquid`) that outputs `og:title` and `og:description`, either:
   - **Remove** that snippet from the layout, or  
   - **Edit** it so it does not output `og:title` / `og:description` (our snippet provides these).
5. Save.

---

## Rules applied

- **Titles** ≤ 60 characters.
- **Meta descriptions** 150–160 characters.
- **Open Graph** `og:title` and `og:description` match the SEO title and meta description.
- Only **existing brand wording** and keywords; no keyword stuffing.
- **No changes** to visible page content.

---

## Template / file changes

| Template / file | Purpose |
|-----------------|--------|
| `snippets/seo-meta.liquid` | **New.** Outputs `<title>`, `<meta name="description">`, `og:title`, `og:description`, `og:url`, `og:image`, `og:type`. |
| `layout/theme.liquid` | **Edit.** Replace default title and meta description with `{% render 'seo-meta' %}`; avoid duplicate OG tags. |

No section or page template files are modified; only layout and the new snippet.

---

## Per-page behaviour

| Page | Title | Meta description |
|------|--------|-------------------|
| **Home** | JustPure India \| A2 Bilona Ghee & Pure Indian Foods | Desi cow milk, traditional bilona method, purity, delivery. |
| **Product** | [Product name] \| Just Pure India | Benefits + traditional preparation + purity (or product meta if set). |
| **About** (`our-story` or `about-us`) | About Just Pure India \| Healthy Choice Wealthy Life | Since 2016, farmer-backed, traditional dairy practices. |
| **Other** | Default `page_title` | Default `page_description`. |

OG tags mirror the same title and description for each case.
