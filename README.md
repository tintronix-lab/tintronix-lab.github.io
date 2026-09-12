# tintronix-lab.github.io

This repo exists for two things GitHub Pages will only serve from the domain
root, plus an `index.html` that redirects that root to the docs site at
`/ThreadMapper/`, because without it the root is a 404 and the App Clip's
invocation URL lands there for anyone who cannot run the Clip.

**Both are generated in the private `ThreadMapperCode` repo and copied here.**
Edit them there — `site/well-known/apple-app-site-association` and
`site/root/h/index.html`, the latter built by `tools/build_handoff_page.py`
from `site/handoff/` — or the next build will quietly undo the change.

| path | what it is |
|---|---|
| `.well-known/apple-app-site-association` | the association file, below |
| `h/index.html` | the page a hand-off sticker opens on anything that is not an iPhone |

## `/h` — the sticker's fallback

Every hand-off sticker ThreadMapper prints carries
`https://tintronix-lab.github.io/h?d=…` with the whole inspection inside the
URL. An iPhone never fetches it: iOS matches the association file and opens the
app, or launches the App Clip. Everything else does — an Android phone, a
laptop, a Mac, any QR reader that hands the URL to Safari — and until
2026-09-12 all of them got a 404 from a sticker on their own hub.

`h/index.html` decodes the same bytes in the browser and draws the same report,
in whichever of the nine languages the reader's browser asks for. It fetches
nothing, it is `noindex`, and it sends no referrer, because that URL is a
customer's home. Pages redirects `/h` to `/h/` and keeps the query; the page
puts the canonical path back so a copied link still opens the app.

GitHub Pages decides who serves `https://tintronix-lab.github.io/` **by repo
name**: only a repo named exactly `<user>.github.io` gets the domain root.
Everything else — including `tintronix-lab/ThreadMapper`, which serves the
public docs site — is a *project* site under a path.

Apple requires an app's site-association file at the domain root, with no path
and no redirect:

    https://tintronix-lab.github.io/.well-known/apple-app-site-association

So the docs repo could not serve it, and this repo was created to.

## What the file does

It is how a domain vouches for an app. ThreadMapper's **App Clip** carries the
matching entitlement `appclips:tintronix-lab.github.io`; both halves must agree
before iOS will open the Clip from a link.

Without it nothing can launch the Clip. Every route in — App Clip Code, QR
code, NFC tag, a link in Messages — is a URL, and iOS checks each one against
this file. When it is missing the link simply opens the website instead: no
error, no prompt, nothing to debug.

## Two things that will silently break it

- **`.nojekyll` must stay.** GitHub Pages runs Jekyll by default, and Jekyll
  drops any directory beginning with a dot — including `.well-known`. Without
  that empty file this repo publishes and the association file does not.
- **No file extension**, and it must be served as `application/json`.

## If the app's identifiers change

The file names `QCSX955Y7P.com.tintronixlab.ThreadMapper.Clip` — the team ID
and the Clip's bundle ID. If either changes, this file is wrong and the Clip
stops being launchable, again with no error anywhere.

Kept in sync with `site/well-known/` in the private ThreadMapperCode repo,
whose README carries the full background.
