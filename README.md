# tintronix-lab.github.io

This repo exists for one file — plus an `index.html` that redirects the domain
root to the docs site at `/ThreadMapper/`, because without it the root is a 404
and the App Clip's invocation URL lands there for anyone who cannot run the Clip.

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
