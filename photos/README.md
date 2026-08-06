# Photos

Drop job photos in this folder, then swap the placeholder blocks in `index.html`.

## What to shoot

**Before/after pairs are the most valuable thing on the site.** Shoot both frames from
the *exact same spot* — same angle, same distance, ideally same time of day. Stand
somewhere you can find again, or mark it with a chalk line.

The single best hero shot is a **half-cleaned driveway** — one pass down the middle so
clean and dirty sit side by side in one frame. It sells the service without a word.

## Suggested names

| File | Where it goes |
|---|---|
| `hero.jpg` | Top of the page — wide, bright, half-clean driveway or house wash |
| `before.jpg` | Left side of the before/after slider |
| `after.jpg` | Right side — same framing as `before.jpg` |
| `og.jpg` | Social share preview, 1200×630 (goes in the parent folder) |

## How to swap one in

Find the placeholder in `index.html`:

```html
<div class="ph"><b>📸</b>Hero photo goes here…</div>
```

Replace the whole `<div>` with:

```html
<img src="photos/hero.jpg" alt="Freshly pressure washed driveway in Houston" loading="lazy">
```

Keep the `alt` text descriptive — it helps local search and screen readers.

## Before uploading

Resize to about **1600px wide** and save as JPG around 80% quality. Photos straight off
a phone are 4–8 MB each and will make the site slow on mobile data, which is exactly
where most of your customers will open it.
