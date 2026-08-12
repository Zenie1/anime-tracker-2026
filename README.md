# 🎌 Anime 2026 Season Tracker

A dynamic web application that tracks anime releases for 2026 by season.

## 🚀 Features
- 🔍 Search anime by title, genre, or studio
- 🎯 Filter by season, status, streaming platform, studio, and minimum score
- 📊 Sort by score, title, or premiere date
- 🖼️ Live poster images via the Jikan API
- ❤️ Local accounts with a watchlist and per-episode "watched" tracking
- 🎬 Spotlight carousel, weekly airing schedule, and trailer playback
- ⚡ Responsive, modern UI with a mobile bottom-sheet layout
- ▶️ "Watch Now" links out to [Anidap](https://anidap.se) for each airing show

## 🛠️ Built With
- HTML5
- CSS3
- JavaScript (Vanilla)
- Jikan API (MyAnimeList)

## 🌐 Live Demo
👉 (Add your Netlify/GitHub Pages link here)

## 📂 Project Structure
Single-file app: `index.html` (styles + markup + script, no build step required).
Just open it in a browser or serve it with any static file host.

## 🔧 What changed in this update

- **Fixed the Jikan API layer.** The real cause of "Failed to load from Jikan
  API" was the *shape of the URL*, not the network. Jikan sits behind a CDN
  whose origin is slow enough that any cache-cold URL times out with a
  **504 Gateway Timeout**. The app requested
  `/seasons/2026/winter?filter=tv&page=1&limit=25` — a URL essentially no
  other client asks for, so it was never warm in the cache, so it 504'd every
  single time. The bare `/seasons/2026/winter` URL is hit constantly by other
  clients, stays warm, and returns 200 instantly. The app now requests the
  bare URL and filters by type client-side (which it was already doing anyway).

  On top of that the fetch layer now: retries 5xx/timeouts with backoff (the
  failed request itself often warms Jikan's cache, so a retry succeeds);
  falls back through refreshed CORS proxies, including a fixed `corsproxy.io`
  URL format that had changed to require `?url=`; keeps partial results
  instead of discarding a whole season when a deep page is cold; and can no
  longer be crashed by a single malformed entry.

- **The app no longer shows an empty error screen when it has usable data.**
  If Jikan is unreachable it falls back to your last saved copy (however old)
  and labels it "Offline copy". A failed background refresh keeps the data
  already on screen instead of wiping it. When it genuinely can't show
  anything, the error now names the actual failure and which seasons failed,
  rather than a generic "Failed to load".
- **Replaced the dead streaming site.** Every "Watch on AniWatch" link
  (aniwatchtv.to) pointed at a site that's been taken down. Watch links now
  point to [Anidap](https://anidap.se) instead:
  - By default, each show links to an Anidap search pre-filled with its title.
  - Anidap is a client-rendered app with no public API, so there's no way to
    auto-resolve a title to its *exact* episode page. Instead, each show's
    modal has a **📌 Set exact link** button — paste the real URL once you've
    found it on Anidap, and the app remembers it (stored in your browser)
    for both the modal's "Watch Now" button and the card's quick-watch
    overlay going forward.
- **Full visual redesign.** New "Aurora" look: dark glass panels, a
  violet → cyan gradient accent, Space Grotesk/Inter/JetBrains Mono
  typography, rounded cards with hover glow, and a reworked hero, spotlight
  carousel, sidebar, modal, and mobile navigation — all existing features
  (auth/watchlist, filters, spotlight, weekly schedule, episode tracking,
  trailers) are unchanged functionally, just restyled.

## ⚠️ Note on Anidap links

Anidap is a third-party streaming aggregator, not something this project is
affiliated with. Links are provided purely as a convenience the same way the
old AniWatch links were — verify content availability/legality in your own
region.
