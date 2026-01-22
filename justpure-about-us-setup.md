# JustPure About Us – Shopify Section Setup

## How to Install

1. **Shopify Admin** → **Online Store** → **Themes** → **Customize** (or **Edit code**).
2. Under **Sections**, click **Add a new section**.
3. Create a new file: `about-us.liquid`.
4. Copy the full contents of `justpure-about-us-section.liquid` into it and save.

**OR** if you prefer to add via Edit code:

1. **Themes** → **Edit code** → **Sections**.
2. **Add a new section** → name it `about-us`.
3. Paste the contents of `justpure-about-us-section.liquid` and save.

## How to Use on Your About Us Page

1. Go to **Online Store** → **Pages** → open your **About Us** page (or create one).
2. In the right sidebar, set **Theme template** to a template that supports sections (e.g. **Default** or a custom **page** template).
3. Click **Customize** (from the page or from Themes).
4. Open the **About Us** page in the preview.
5. **Add section** → choose **About Us**.
6. Place it where you want and **Save**.

## Customizing Content

All main text, headings, and CTA are editable in the theme editor:

- **Hero**: title, subtitle, short intro.
- **Our Story**: heading, story content (rich text), optional image.
- **Why Just Pure**: Authentic, 100% Quality, Pocket Friendly titles and descriptions.
- **Business Model**: Production, Milk Collections, Processing titles and text.
- **Vision & Promise**: headings (list items are in the Liquid; can be moved to schema if you want them editable).
- **Milestones**: heading (achievement text is in the Liquid).
- **CTA**: heading, supporting text, button label, and link.

Default content matches the [JustPure About Us](https://justpureindia.com/about-us/) copy. Update any of it via **Customize** → **About Us** section.

## Design Notes

- **Premium typography**: Cormorant Garamond (serif) for headings, DM Sans for body. Google Fonts loaded in-section.
- **Larger, more spacious layout**: 1520px max-width, 100–120px section padding, bigger type throughout.
- Green gradient hero and CTA; warm off-whites (`#f8f6f3`) for alternating sections.
- Responsive: stacks nicely on mobile (values, business model, milestones, vision/promise).
- Section-specific CSS scoped to `#about-us-{{ section.id }}` to avoid clashes with your theme.
- Optional **Story image** in **Our Story**; if none is set, that block is hidden and the layout still works.

## Fallbacks

- If **CTA button link** is empty, the button goes to `{{ routes.root_url }}collections/all`.
- **Shop Now** is used when the CTA button label is blank.
