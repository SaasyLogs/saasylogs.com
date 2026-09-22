# Saasy Logs — website

Public landing page for [saasylogs.com](https://saasylogs.com). Served by GitHub
Pages from the repository root.

**The parser engine is not in this repository.** It lives in a separate private
repo. See [Repository layout](#repository-layout).

---

## Contents

```
index.html          Landing page. No build step, no dependencies.
styles.css          All of it. Plain CSS, no preprocessor.
CNAME               Custom domain for GitHub Pages (saasylogs.com).
LICENSE             All rights reserved. See the file.
brand/
  logo.svg          SL monogram. Use at 64px and up.
  favicon.svg       Sunglasses only. Use below 64px — see note below.
  favicon.ico       16/32/48 fallback for browsers that ignore SVG icons.
  favicon-*.png     16, 32, 48, 180 (apple-touch-icon).
  logo-*.png        128, 256, 512 raster exports for stores and social cards.
```

### Why there are two marks

They answer different problems, so they get different answers.

`logo.svg` is the SL monogram on a Marigold tile with a Copper keyline, letters
knocked out in Chalk. The keyline lives **in the file**, not in CSS, because
Google, Slack, LinkedIn and email clients all render this on backgrounds nobody
here chooses — and a mark that only works on saasylogs.com is a mark that fails
everywhere it matters. The letters are baked outlines, not `<text>`, so it does
not depend on the viewer having a font.

`favicon.svg` is the sunglasses alone, Soot on Marigold. The monogram and the
glasses compete for the same pixels, so below ~48px neither resolves; and a
keyline that is 4% of the width is sub-pixel at 32px and turns to mush. The
small mark takes its contrast from the tile instead — 8.65:1.

Both were verified by rendering at 16/32/48/128px rather than by eye at 100%.

Do not "fix" the favicon by putting the letters back, and do not give the logo
its tile back in CSS.

### Changing an icon

Bump the `?v=` on every icon URL in `index.html`. Browsers cache favicons past
a hard reload and sometimes indefinitely; without the bump, anyone who has seen
the old one keeps it. This is how the blue mark survived its own replacement.

---

## Local preview

No toolchain required.

```bash
python3 -m http.server 8000
# open http://localhost:8000
```

Paths in `index.html` are root-absolute (`/brand/...`), so opening the file
directly with `file://` will not load the logo. Use the server.

---

## Repository layout

| Repo | Visibility | Contents |
|---|---|---|
| `SaasyLogs/saasylogs.com` | public | this site |
| parser repo | **private** | `Parser.gs`, `Render.gs`, `Main.gs`, `Story.gs`, `Paste.html`, `Scrub.gs`, tests, fixtures |

The split is deliberate. The parser is the product; publishing it is a one-way
door. The site can be public because it is marketing.

---

## Committing safely

Debug logs are the hazard in this project. A single Apex log can contain record
IDs, email addresses, field values and the shape of an org's private automation.
Once pushed to a public remote it is permanent — force-pushing does not remove
it from the GitHub API, from forks, or from anyone's existing clone.

Two layers guard against this, in **both** repos:

**1. `.gitignore`** ignores every `*.log` by default. Only
`fixtures/*.fixture.log` — output that has been through the scrubber — is
allowed through.

**2. A pre-commit hook** catches deliberate mistakes that `.gitignore` cannot:
a `git add -f`, an ID pasted into a README, a key in a config file. Install it
once per clone:

```bash
cp pre-commit .git/hooks/pre-commit
chmod +x .git/hooks/pre-commit
```

It blocks raw `.log` files, Salesforce record IDs, and private keys or client
secrets in added lines. It warns on email addresses and files over 2 MB.
`fixtures/*.fixture.log` is exempt from the ID check, because the scrubber
keeps record IDs on purpose, so every fixture is full of ID-shaped strings —
and a guard that fires on every commit is a guard people learn to bypass with
`--no-verify`. Tests use the nine-consecutive-zeros placeholder form instead,
which the hook also exempts and which reads as obviously synthetic.

### Making a fixture

Never commit a log straight from an org. Run it through the scrubber first:

```bash
node Scrub.gs raw.log fixtures/case-name.fixture.log --report
```

The scrubber is deterministic: the same input and seed always produce
byte-identical output, so re-running it does not churn the diff.

**It replaces the data and keeps the shape.** Gone by default: emails, phone
numbers, URLs, quoted literals, `field=value` payloads, `USER_DEBUG` bodies,
and any field whose name says it holds a person — `MobilePhone`,
`MailingStreet`, `SBQQ__BillingName__c`, `Contact_Mailing_Address__c`.
Credentials and tokens go unconditionally and cannot be kept.

**Kept on purpose:** record IDs, class, method, field and object names,
workflow-rule names, flow element names, namespaces, and every timing,
nanosecond counter, row count, limit number and stack depth. Those are what
make a log readable, and an ID resolves only for somebody who already has
access to the org it came from.

That split is the whole design. An earlier version replaced record IDs and
aliased Apex names; the result was unreadable and protected nobody.

The blanket replacement of quoted and variable values stays on by default
because no field-name list is ever complete — it is the backstop for the
custom field nobody thought to name.

---

## Deployment

Push to `main`. GitHub Pages serves the root. `CNAME` holds the custom domain;
HTTPS is provisioned by GitHub.

DNS lives at Porkbun:

- four `A` records on `@` → `185.199.108.153`, `185.199.109.153`,
  `185.199.110.153`, `185.199.111.153`
- one `CNAME` on `www` → `saasylogs.github.io`

**Typo domains** (not served here): sassylogs.com, saasylog.com and
sassylog.com use Porkbun URL forwarding to `https://saasylogs.com` with a
permanent 301, path included, wildcard on.

---

## Licence

All rights reserved. See [LICENSE](LICENSE). Not affiliated with Salesforce,
Inc. or Google LLC.
