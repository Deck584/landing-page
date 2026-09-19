# Declan Sweeney personal landing page

Single-page static site for a senior sales leader. No build step, no framework,
no dependencies.

## Layout

- `senior_sales_leader_personal_landing_page_declan_sweeney.html` is the whole
  site: inline CSS (design tokens on `:root`), semantic sections, and a short
  inline script for smooth scrolling, the footer year, and the resume button.
- `README.md` is a one-line description. There is no package.json, no tests,
  and no CI.

## Preview

Open the HTML file directly in a browser, or serve the folder:

```
python3 -m http.server 8000
```

Then visit http://localhost:8000/senior_sales_leader_personal_landing_page_declan_sweeney.html

## Deploy

Any static host works (GitHub Pages, Netlify, Vercel). If publishing to a root
URL, copy or rename the HTML file to `index.html` in the same commit rather
than duplicating content in two files.

## Editing rules

- Keep it one file. Do not split CSS or JS out, add a bundler, or pull in a
  framework or CDN library for a change this page does not need.
- Change colours, radii, and widths through the `:root` tokens, not by adding
  new hard-coded values.
- Keep the section order: header, hero, key metrics, approach, achievements,
  experience, testimonials, resume CTA, contact, footer. Anchor links in the
  nav depend on the section ids.
- Preserve accessibility attributes (`aria-label`, `rel="noopener"` on
  external links, semantic headings in order).
- The page must stay readable on a phone with no horizontal scroll.

## Copy and facts

- The metrics (portfolio size, account count, growth, new ARR) are personal
  career claims. Do not alter, round, or invent numbers. If a change needs a
  new figure, ask for it.
- Keep testimonials verbatim. Do not generate new ones.
- Tone is direct and senior. No buzzword padding, no emoji.

## Known placeholders

These are not yet real and should be replaced by the owner, not guessed:

- LinkedIn URL uses `USERNAME` (two places).
- Email uses `declan@example.com` (two places).
- The resume button has no file behind it and shows an alert on click. When a
  PDF is added, point the button `href` at it and delete the alert handler.
