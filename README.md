# memorialbros.com

Static one-page site for Memorial Bros — pressure washing & exterior cleaning, Houston TX.

No build step, no dependencies, no CDN. Open `index.html` in a browser and that's the site.

## Files

| File | What it is |
|---|---|
| `index.html` | The entire site — HTML, CSS and JS in one file |
| `404.html` | Not-found page (Cloudflare Pages serves this automatically) |
| `logo.png` | The logo with its white background removed, 383×198 transparent |
| `favicon.png` | 180×180 browser/app icon — logo on a brand-red tile |
| `favicon-32.png` | 32×32 classic tab icon |
| `og.jpg` | 1200×630 social share card |
| `robots.txt` | Lets search engines in, points at the sitemap |
| `sitemap.xml` | One URL. Update if pages get added |
| `photos/` | Drop real job photos here |

## Brand colors

Sampled straight out of the logo, not eyeballed:

| | Hex | Used for |
|---|---|---|
| Red | `#b70205` | Call buttons, accents, the diagonal slash. 6.9:1 with white text |
| Black | `#101014` | Hero, header, footer, quote section |
| White | `#ffffff` | The wordmark, body backgrounds |

Everything derives from `--red` in the `:root` block at the top of `index.html`.
Change that one value and the whole site follows.

### About the logo file

The supplied logo was a 398×224 JPEG-quality PNG on a solid white background. The
background was removed with a **border-seeded flood fill**, not a global white-to-alpha
swap — the wordmark is white, so a naive removal would have punched holes through
"MEMORIAL BROS". Only white connected to the image edge was cleared; the black outlines
protected everything inside. Edge pixels got proportional alpha so there's no pale
fringe on dark backgrounds.

⚠️ The source is only 383×198 after trimming. That's plenty for the header and favicon,
adequate for the share card, and **too small for print** — truck decals, yard signs,
shirts. Get the original vector (`.svg` / `.ai` / `.eps`) from whoever made it before
ordering anything physical.

## Contact details

Phone `(417) 414-8650` · Email `memorialbros@gmail.com`

These appear in several places (header, hero, contact list, footer, mobile call bar,
the SMS fallback in the form, and the JSON-LD schema block). To change them,
find-and-replace all three formats:

- `(417) 414-8650` — display text
- `+14174148650` — `tel:` links
- `+1-417-414-8650` — the schema block only

## Contact strategy

**Email is the primary call-to-action.** The red buttons in the header, hero, and
mobile bar all open an email. The phone number is still on the page — in the contact
list and the footer — but it's secondary.

To flip back to phone-first, swap the `mailto:` hrefs on `.btn-call` back to `tel:`.

## The quote form

`FORM_ENDPOINT` at the bottom of `index.html` is empty on purpose. With no endpoint,
submitting the form opens a **pre-filled email** to `BUSINESS_EMAIL` with the customer's
name, phone, address, service and notes already written out — and prompts them to attach
a photo. No third-party service and nothing to pay for.

To collect submissions server-side instead, paste a Formspree endpoint into
`FORM_ENDPOINT` and the form posts JSON to it.

## Pricing

The `#pricing` table comes from the **Suggested quote** column of the internal quoting
model. The underlying rates ($/sq ft, crew-hour rates, productivity, hourly floors) are
deliberately **not** published — only the customer-facing quotes and the percentage
adjustments.

If the quoting model changes, update the table in `#pricing`.

## Still to do

- [ ] Real photos — the before/after pair in `#results` matters most.
      Shoot both frames from the exact same spot.
- [ ] Real reviews — `#reviews` has bracketed placeholders. Do not publish invented ones.
- [ ] Confirm the neighborhood list in `#area` is where you actually work.

## Deploy

Cloudflare Pages. No build command, output directory `/`.

Custom domain `memorialbros.com` — nameservers moved off Wix to Cloudflare.
Registrar is still Wix until the ICANN 60-day lock expires (~2026-09-24), after which
the domain can be transferred to Cloudflare Registrar.
