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

- **Turn off Link In Bio first.** Porkbun enables it on every newly registered
  `.com`, which auto-redirects the domain to `yourdomain-com.l.ink` and serves
  a Porkbun landing page. It holds the domain, so a URL forward configured
  underneath it does nothing. Symptom: the domain resolves, redirects, and
  lands on "A Brand New Domain! Brought to you by Porkbun."
- **Choose permanent (301).** Porkbun defaults to a temporary 302/307. Their
  reasoning is that it's reversible and doesn't affect the *target's* SEO,
  which is fine for a domain you might repoint — but these are permanent
  typo-catchers. A 302 leaves them eligible to be indexed as separate sites;
  a 301 consolidates everything onto `saasylogs.com`.
- **Never masked/frame forwarding.** It keeps the typo domain in the address
  bar and serves the site inside a frame, so link previews read the wrapper
  instead of the page and every share loses the `og-image.png` card.
- **Forward the path, not just the root**, so `sassylogs.com/anything` lands on
  `saasylogs.com/anything` rather than dumping everyone on the homepage.
- `www.saasylogs.com` needs no forward. GitHub redirects `www` to the apex
  automatically once the DNS records exist.

**Verify by fetching, not by trusting the settings screen.** As of 22 Sept all
three still returned the Porkbun parking page after the forwards were believed
set. A saved form is not a working redirect.

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
