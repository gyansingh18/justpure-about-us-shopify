# Fonts preconnect & async load (PageSpeed)

This reduces **critical path latency** and helps fix **LCP Unscored** / "Avoid chaining critical requests" in PageSpeed Insights (mobile).

## What changed

- **Before:** Each custom section used a blocking `@import url('https://fonts.googleapis.com/...')` inside `<style>`. That chained after the theme’s `compiled_assets/styles.css` (~1.9s) and then loaded Google Fonts CSS and WOFF2 files (~2s), so maximum critical path was ~2,054 ms.
- **After:** Fonts are loaded once from the layout via the `fonts-preconnect` snippet: **preconnect** to `fonts.googleapis.com` and `fonts.gstatic.com`, and the font stylesheet is loaded **asynchronously** (`media="print"` + `onload="this.media='all'"`) so it doesn’t block first paint.

## Setup (required)

1. **Upload** `snippets/fonts-preconnect.liquid` to your theme (if not already there).
2. **Edit** `layout/theme.liquid` in the theme editor.
3. In the `<head>`, add the snippet **as early as possible** (e.g. right after `<meta charset>` and `<meta name="viewport">`):

   ```liquid
   {% render 'fonts-preconnect' %}
   ```

4. Save. Re-run PageSpeed Insights (mobile) to confirm lower critical path latency and that LCP can be scored.

## Notes

- The snippet uses the same font families as the custom sections: **Cormorant Garamond**, **DM Sans**, **Playfair Display** (`display=swap`).
- If the snippet is not included in `theme.liquid`, custom section pages will still render but will fall back to `var(--font-body-family)` / system fonts until the theme or another asset loads these fonts.
