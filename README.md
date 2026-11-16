# Zendex

**Live tech news, in one clean feed.**

Zendex is a lightweight tech news reader: it pulls the latest stories from the public RSS feeds of the biggest technology publications and shows them as a single, fast, distraction-free feed. It is one HTML file — no framework, no build step, no dependencies, no accounts, no tracking.

---

## Sources

Stories are read live from each publisher's own public RSS/Atom feed:

| Source | Feed |
| --- | --- |
| The Verge | `theverge.com/rss/index.xml` |
| TechCrunch | `techcrunch.com/feed` |
| Ars Technica | `feeds.arstechnica.com` |
| Wired | `wired.com/feed/rss` |
| Engadget | `engadget.com/rss.xml` |
| Hacker News | `hnrss.org` (front page, 150+ points) |
| MIT Technology Review | `technologyreview.com/feed` (included, off by default) |

You can switch any source on or off, and add any other RSS or Atom feed — in any language — from **Settings → Add an RSS feed**.

Headlines and short summaries come from the feeds; **Read full story** opens the article on the publisher's site.

## Features

- **One merged feed** — every story from every enabled source, newest first, with relative timestamps and day grouping.
- **Source filter** — see one publication at a time, plus a per-source list with live status (loading, loaded, or why it failed).
- **Read view** — a bottom sheet with the story's image, title, author, summary and topic tags; tap it to expand to full screen, swipe down to dismiss.
- **Reader mode** — warm-paper background and a serif face, with four text sizes.
- **Saved stories** — bookmark any story; saved items stay on the device and are readable offline.
- **Search** — instant search across loaded and saved stories by title, summary, source and topic, with recent searches and trending topics. Press `/` to open it.
- **Topic tags** — stories are automatically tagged from their text: AI, Apple, Google, Microsoft, Security, Chips, Space, EVs, Gaming, Funding, Policy, Social. Tap a tag to search it.
- **Light / dark / system theme**, following the OS by default.
- **Always fresh** — pull to refresh, an automatic refresh every 10 minutes while the app is open, and a "N new stories" pill when something arrives.
- **Works offline** — the last feed is cached on the device and shown when the network is unavailable, with automatic retries and backoff.
- **Right-to-left ready** — Persian/Arabic headlines render correctly (the Vazirmatn font is loaded for them).
- **Installable** — a web app manifest is embedded in the file, so it can be added to a phone's home screen and run full-screen.
- **Keyboard and screen-reader friendly** — focus trapping in overlays, live-region announcements, skip link, roving focus.

## Get it

**Android app (v1.0.0)** — download the APK from the release page:

➡️ **https://github.com/mohammedaminsalmany/Zendex/releases/tag/v1.0.0**

Open the downloaded `zendex_v1.0.0.apk` on the phone and allow installation from your browser/file manager when Android asks (the app is distributed here directly, not through Google Play). No account or sign-in is needed. The Android build is a thin WebView wrapper around the hosted web app, so the reading experience is the same as in the browser.

**Web** — `index.html` is the whole app. Open it in a browser, or put it on any static host (GitHub Pages, Netlify, an S3 bucket, your own server) and open the URL on a phone. Add it to the home screen from the browser menu to get it full-screen.

## How it works

Everything lives in `index.html`: markup, styles and a single vanilla-JavaScript IIFE (~1,800 lines), no external libraries.

- **Fetching.** Browsers can't read most publishers' feeds directly (CORS), so Zendex tries several delivery routes for every feed and keeps the first that works: a same-origin `rss.php?url=…` proxy (used when you host the optional helper script), then a direct request or the rss2json service, then public CORS proxies (allorigins, codetabs, corsproxy.io). Each route has a 10-second timeout; failures are reported per source in Settings and retried with backoff (3, 6, 12, 20, then every 30 minutes).
- **Parsing.** RSS 2.0, RSS 1.0/RDF and Atom are supported, in both XML and the JSON shape returned by feed services. Titles, links, dates, authors, images, summaries, categories and Hacker News discussion links are extracted, HTML is flattened to text, and only `http(s)` links are kept.
- **Merging.** Stories are de-duplicated by a hash of their link and merged newest first; later feeds that finish loading are merged in without disturbing what you're reading.
- **Local state.** Everything you change is stored in the browser's `localStorage` under the `zendex.*` prefix — feed toggles, custom feeds, saved stories, read stories, theme, reader settings, recent searches and the story cache. Nothing is sent anywhere; there is no backend, no analytics and no cookies.

## Hosting notes

Any static host works; the file needs no server-side code. To make feed loading as reliable as possible, you can add a small same-origin proxy at `rss.php?url=<feed>` that fetches the feed and returns it as XML with an `Access-Control-Allow-Origin` header — Zendex tries that route first and falls back to public proxies when it isn't there.

## Repository layout

```
index.html    the entire app (HTML, CSS, JavaScript)
README.md     this file
```

## Credits

All stories, images and trademarks belong to their publishers — The Verge, TechCrunch, Ars Technica, Wired, Engadget, Hacker News and MIT Technology Review. Zendex only reads their public feeds and links back to them.

Vazirmatn font by Saber Rastikerdar (SIL Open Font License), loaded from jsDelivr.
