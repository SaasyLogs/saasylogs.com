# saasylogs.com — marketing site

Project rules for the public site. The parent `CLAUDE.md` (how we work) still
applies, including branching and versioning — not restated here.

Product decisions, the parser, the backlog and anything unannounced live in the
private `SaasyLogs/App` repo. Not here.

## This repo is public

Everything committed here, including history, is visible to anyone.

- No keys, tokens, or credentials.
- No internal notes, and **no identifiers from a work org.** Class and method
  names from Randi's employer reached this site once and had to be scrubbed
  after the fact. Every code example on this page is invented or comes from a
  scrubbed fixture. Check before pushing.
- A mistake in history is public even after it's deleted.

## How it's built

- Plain static HTML/CSS served by **GitHub Pages** from `main`. No build step
  until there's a reason for one.
- **Merging to `main` is publishing.** There is no separate deploy to think
  twice during.
- `styles.css` is a real stylesheet, not a `<style>` block — the page was 697
  lines before the CSS came out of it.
- The title font is self-hosted from `/fonts`. See `brand/README.md`; the short
  version is that the old stack named a font almost nobody has and silently
  fell back to Georgia.

## Domains

**`saasylogs.com` is canonical.** It's what the `CNAME` file holds (GitHub
Pages creates that file — don't delete it), what `<link rel="canonical">` and
the `og:` tags point at, and the only one GitHub serves.

Also owned, all forwarding at the registrar:

| Domain | Why |
|---|---|
| `sassylogs.com` | **The likely misspelling.** "Sassy" is the real English word; "Saasy" is the SaaS pun. People will type the correct English and be wrong. |
| `sassylog.com` | Both variations at once — misspelled and singular |
| `saasylog.com` | Correct spelling, singular |

**GitHub Pages serves exactly one custom domain per repo** (the apex plus its
`www`), so these cannot be added to `CNAME` or to Pages settings. They forward
at the registrar and that is the only place they exist.

When setting the forwards:

- **301 permanent, not masked/frame forwarding.** A masked forward keeps the
  typo domain in the address bar and serves the site inside a frame: link
  previews then read the wrapper instead of the page, so every share of a
  masked URL loses the card built in `og-image.png`. A 301 hands the visitor
  and the crawler to the real domain and consolidates the SEO.
- **Forward the path, not just the root**, so `sassylogs.com/anything` lands on
  `saasylogs.com/anything` rather than dumping everyone on the homepage.
- `www.saasylogs.com` needs no forward. GitHub redirects `www` to the apex
  automatically once the DNS records exist.

## Brand

`brand/README.md` is the source of truth: the mark, the Ember palette with
measured contrast ratios, type, and what every file is for. Read it before
changing an icon — the sunglasses position in particular is a tested value,
not a taste call, and there's a note about judging it at 1:1.

**Cache-bust when an icon changes.** Every icon URL carries `?v=N`.

## Voice

- **Free. Not a free tier**, not a trial. There is no paid edition.
- Salesforce-specific, and says so. Don't hedge it into "log tooling".
- Lead with the path the code took, not with timings.
- **No invented numbers** — no user counts, no "X% faster", no savings claims.
  The product's argument is that tools hand you confident numbers they can't
  stand behind; the site cannot then do it.
- Promote what it does. Don't argue with feedback nobody gave.

## Known gaps

- Protect `main` in repo Settings → Rules (free for public repos): require a
  PR, no direct pushes.
- The repo is still on the pre-`release/vN` branching model — see the migration
  note in `SaasyLogs/App/CLAUDE.md`.
- No analytics, by choice. The product's stance on customer data starts here,
  so decide deliberately before adding any.
