# Saasy Logs brand

## Mark

An **S and L monogram wearing sunglasses**. The letters are outlines drawn from
DejaVu Serif Bold, never live `<text>` — a logo that needs a font installed
renders differently on every machine.

`logo.svg` is the source of truth for **64px and up**. `favicon.svg` is the
small mark and is the sunglasses alone.

### Why there are two files

The glasses and the interlocked SL compete for the same pixels. Below 64px the
full mark turns to an indistinct blob, so the small mark keeps the glasses —
the distinctive half — and drops the letters. Different problem at that size,
different answer.

### Why the glasses come back the way they do

They were removed on 19 Sept because they covered the S's crossing and the L's
spine. That was correct, and it was re-tested on 21 Sept rather than argued
about: rendered across the bowl, the S disappears entirely at 128 and 64 and
you are left with glasses and an L.

### The height is not a free parameter

The first fix overcorrected. Glasses at `y=172` overlapped only 22 units of a
170-unit letter and read as floating above the S rather than worn on it.
Randi, 21 Sept: *"no I do not like the sunglasses that high."*

Shipped position is **`y=232`**, the lowest rung of a 172 / 190 / 205 / 218 /
232 ladder rendered at 512 and 64. Lens bottom lands at `y=275`. The S's
**crossing** — the stroke that makes an S an S — runs through roughly
`y=270–290`, so the glasses reach it and stop. **That is the floor.** Below
this the letter goes, and the 19 Sept failure returns.

**Judge this at 1:1, never from a contact sheet.** 232 was first written off
as "the S's top is eaten at 64px and the halo degrades into a stray orange
line." That was read off a small montage tile and it was wrong: zoomed at 64
the S reads, and the pale line above the glasses is the S's own white crown,
not a halo artefact. A wrong rendering claim nearly cost the version that got
picked.

If you move the glasses, re-render the ladder.

Three things hold it up. Remove any one and the mush returns:

1. **Glasses reach the crossing and stop.** They never cross it.
2. **Letters dropped to baseline `y=360`** (from `337.65`) for headroom.
3. **The Marigold halo.** A 16-unit tile-coloured stroke behind the glasses
   cuts a gap between black glass and white letter. Without it two dark-on-
   light shapes fuse at small sizes. At 64px it is the only reason the S reads
   at all — and it is also what bought the room to sit this low.

Verified at 512 / 128 / 64. At 32 it does turn to mush, which is what
`favicon.svg` is for.

**Anything embedding the logo regenerates with it:** `logo-128/256/512.png`,
`avatar-512.png`, `/og-image.png`, `github-social-preview.png`.

## Palette: Ember

| Token | Hex | Use |
|---|---|---|
| Marigold | `#f59e0b` | tile, brand colour, accents |
| Copper | `#e2622a` | keyline, "Logs", headline emphasis |
| Ember | `#c1362b` | dialog accent, warnings, the gap line |
| Soot | `#17120f` | dark backgrounds, sunglasses |
| Chalk | `#faf6f3` | letters, text on dark |

**Contrast, measured:**

- Chalk on Marigold — **2.00:1**. Below WCAG for text, which is why it is only
  used for heavy letterforms at 128px and up, never for words.
- Soot on Marigold — **8.65:1**. What `favicon.svg` uses, because at 16px
  weight stops compensating for contrast.
- Copper on Chalk — **3.25:1**. Large text only.

Every colour in the logo is **fixed**. An earlier version switched with
`prefers-color-scheme`; it was reverted because the file gets rendered by
Google, Slack, LinkedIn and every email client on backgrounds nobody here
chooses, and a mark that changes by context is a mark nobody recognises.

## Type

- **Headings and wordmark: DejaVu Serif Bold**, self-hosted from
  `/fonts/DejaVuSerif-Bold-subset.woff` (24 KB, Latin-1 plus the punctuation
  the page uses). License in `/fonts/LICENSE-DejaVu.txt`.
- **Body: system UI stack.** No webfont for body text.
- **Never loaded from Google Fonts.** The site makes no third-party requests.

**DejaVu is an availability decision, not a design one.** EB Garamond was the
first choice and Merriweather the second; neither was installed on the machine
the logo was drawn on and apt needed root. If the face is ever reconsidered,
the logo's two `<path>` elements regenerate from the new `.ttf` and nothing
else in the file moves — but the webfont and every PNG below regenerate too.

WOFF rather than WOFF2 for the same reason: no brotli available. WOFF2 would
be ~30% smaller; regenerate with `--flavor=woff2` anywhere that has it.

## Voice

- **Free. Not a free tier**, not a trial. There is no paid edition, so nothing
  should be written as though there is.
- Salesforce-specific, and says so. Don't hedge it into "log tooling".
- **No invented numbers** — no user counts, no "X% faster", no savings claims.
  The product's whole argument is that tools hand you confident numbers they
  can't stand behind.
- Lead with the path, not the timings.

## Files

| File | Where it goes |
|---|---|
| `logo.svg` | Source mark, 64px and up |
| `logo-128/256/512.png` | Raster fallbacks |
| `favicon.svg` | Site favicon, 16–32px — glasses only |
| `favicon-16/32/48/180.png`, `favicon.ico` | Legacy icon requests |
| `/apple-touch-icon.png` | iOS home screen (copy of favicon-180) |
| `avatar-512.png` | GitHub org avatar, profile pictures |
| `github-social-preview.png` (1280×640) | Repo Settings → Social preview |
| `github-profile-README.md` | Copy to the org's `.github` repo |
| `/og-image.png` (1200×630) | Link previews for saasylogs.com |
| `social/*.svg` + `.png` | Per-post blog and LinkedIn cards |

**Cache-bust when an icon changes.** Every icon URL carries `?v=N`; bump it, or
anyone holding the old one never sees the new one.
