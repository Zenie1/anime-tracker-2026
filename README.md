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

- **Fixed the Jikan API layer.** `corsproxy.io` had changed its URL format
  (now needs `?url=`) and the fallback proxies had gone stale, so any hiccup
  in the direct fetch would leave the app stuck on the error screen even
  though the Jikan API itself was healthy. The fetch layer now tries a
  direct request first, retries on rate-limiting (HTTP 429) with backoff,
  and falls back through a refreshed set of CORS proxies. A single malformed
  entry from the API can no longer crash the whole load.
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
