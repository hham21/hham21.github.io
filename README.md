# hham21.github.io

Public documents for my apps — privacy policies and support pages — served as a
static site at <https://hham21.github.io/>.

There is no build step. The HTML files are served exactly as written, so
checking a change means opening the file in a browser and committing.

## Layout

```
index.html                Index (app list)
style.css                 Shared stylesheet — the single source of style
hanoi-rush/privacy/       개인정보처리방침 (Korean)
hanoi-rush/privacy/en/    Privacy Policy (English)
hanoi-rush/support/       지원 (Korean)
hanoi-rush/support/en/    Support (English)
```

Korean sits at the bare path and English under `en/` because the Korean URLs
shipped first and are baked into released app binaries. That ordering cannot be
swapped now — see the third rule below.

## Rules

- **Use relative links.** Absolute paths (`/hanoi-rush/…`) work once deployed but
  break when a file is opened locally, which is the whole reason there is no
  build step.

- **Wrap every `<table>` in `<div class="table-scroll">`.** Which table overflows
  changes whenever the wording does, so this is a rule rather than a per-table
  judgement. Do not put `overflow-x` on the table itself: that needs
  `display: block`, which breaks table semantics for screen readers.

- **Do not move these paths.** The URLs are baked into App Store Connect and into
  the app binary (`scripts/policy_links.gd` in the hanoi-rush repo), and a binary
  keeps its copy in every released version. Fixing all three places and shipping
  a new build still leaves the button on any non-updated install opening a 404
  forever — and because that is a blank page rather than a crash, the user never
  learns they missed the policy.

- If a path truly must move, **leave a meta refresh stub `index.html` at the old
  path** so older builds keep working. No build step needed:

  ```html
  <meta http-equiv="refresh" content="0; url=/new/path/">
  ```

- The "what Google collects" table in the privacy policy is copied from Google's
  own disclosure (`developers.google.com/admob/ios/privacy/data-disclosure`).
  Google updates that page as the SDK changes, so re-check the table whenever the
  ads SDK is upgraded. It also has to match the App Privacy answers in App Store
  Connect — a mismatch is itself grounds for a 5.1.1 rejection.
