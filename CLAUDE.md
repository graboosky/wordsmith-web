# Wordsmith — Web

Grow your vocabulary and learn to speak clearly and confidently in your own language.

The web platform repository of the Wordsmith container. `../CLAUDE.md` governs product
behavior, domain vocabulary, user-facing copy, and releases. This file governs how the site
is built and served.

**This site exists for one job: the public pages the App Store requires.** It is not an app
and not a mirror — it implements none of Wordsmith's behavior and has no layers. A feature
request that lands here is almost certainly meant for the app.

**This repository becomes public the day Pages is turned on.** GitHub Pages on the free plan
serves only public repositories, so write everything here — this file included — as if it
were already readable by anyone. Nothing that is not meant for the world goes into it: no
drafts of unreleased features, no internal notes, no credentials.

## The tree

```
wordsmith-web/
├── CLAUDE.md
├── README.md
├── .nojekyll     Pages serves files as they are
├── style.css     every colour, size and font, as custom properties
├── index.html    the landing page
├── privacy.html  the privacy policy (App Store Connect: Privacy Policy URL)
└── support.html  support and contact (App Store Connect: Support URL)
```

**This tree is the single source of truth for this repository's structure. When a file or
folder is added, removed, or renamed, update it in the same change.**

## Status

The pages are written (2026-09-30). The repository is still private and Pages is off: going
public is the owner's step (the commands below).

**Stack decided:** plain HTML and CSS, no build step. What is in the repository is exactly
what is served.

**Host decided:** GitHub Pages, deployed from branch `main`, path `/`.

**First task:** write the pages below, add an empty `.nojekyll` at the root so Pages serves
files as they are rather than running them through Jekyll, then make the repository public
and turn Pages on — in that order, because Pages refuses a private repository on the free
plan:

```bash
gh repo edit graboosky/wordsmith-web --visibility public --accept-visibility-change-consequences
gh api -X POST repos/graboosky/wordsmith-web/pages -f 'source[branch]=main' -f 'source[path]=/'
```

Read the whole history before the first command: going public publishes every commit, not
just the current tree.

## The pages the store requires

| Page | Required by | When |
|---|---|---|
| Privacy policy | App Store Connect | every app, before the first submission |
| Support | App Store Connect (Support URL) | every app |
| Terms of use | App Store Connect | only if Wordsmith sells an auto-renewable subscription — Apple's standard EULA may be linked instead |

The pages are `privacy.html` and `support.html`; the terms are Apple's standard EULA, linked from
the paywall. **These names are permanent**: a URL pasted into a store listing is permanent in
practice — renaming a page later means a 404 in front of review and a metadata edit in the
store. The app links them from `wordsmith-ios/Wordsmith/service/account/WordsmithLinks.swift`.

The privacy policy is written from `../docs/domain.md` *What stays on the phone, and what leaves
it*. Contact is through this repository's GitHub Issues until the owner chooses an address.

**The privacy policy must say what the app actually does with what the user gives it** —
above all the voice: whether speech is recorded, whether it is recognized on the device or
sent to a server, whether any recording is kept, and for how long. Write it from
`../docs/domain.md` once the data a feature stores is decided there, and update it in the
same release as any feature that changes what is collected or where it goes.

## Domain

| What | Value |
|---|---|
| Production URL | `https://graboosky.github.io/wordsmith-web/` |
| DNS / registrar | none — the domain is GitHub's. A custom domain is a later decision (a `CNAME` file plus DNS). |
| Host | GitHub Pages |

Every URL a store listing links to must return 200 before any store submission — review
fetches them, and a 404 costs a review cycle. `../docs/release.md` puts web first in the
order for this reason. Check with the verb review uses — a `GET`, not a `HEAD`, which a CDN
can answer differently:

```bash
curl -s -o /dev/null -w '%{http_code}\n' https://graboosky.github.io/wordsmith-web/<page>.html
```

## Build & test

There is no build. Preview locally from the repository root:

```bash
python3 -m http.server 8000
```

then open `http://localhost:8000/`. Once Pages is on, a push to `main` is a deploy.

## Generated content

If any page here is generated from another source — legal text, pricing, release notes —
say so **on the page and here**, with the command that regenerates it. A file silently
overwritten by an export script is a file someone will edit by hand and lose.

- None yet.

## Conventions

- Every colour, size and font is defined once, as a custom property in one stylesheet, and
  pages use the names. The app's `designSystem/` rule, in the only form a static site needs.
- User-facing copy comes from `../docs/domain.md`. Writing a second version of a sentence
  here is how the product starts saying two different things.
