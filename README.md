# ScrapeKit

A native desktop media scraper for macOS and Windows, powered by TMDB.

## Architecture
- **UI:** Flutter desktop (macOS + Windows)
- **Core engine:** Rust, bridged to Flutter via `flutter_rust_bridge`
- **CI:** GitHub Actions builds and packages both platforms on every push

## Scraping pipeline
1. Scan library folders, with file watching for incremental updates
2. Clean filenames (full-width symbols, release-group and fansub tags)
3. Parse with regex rules: `S01E02`, `1x02`, `EP02`, `第02集`, year, resolution
4. Match against TMDB, then group multiple versions (4K / 1080p / Director's Cut) under one entry

## TMDB rate limiting
- Global token bucket (~40 req/s)
- On `429`, respect `Retry-After`, then exponential backoff with jitter
- Search results cached locally for 7 days

## Storage (dual track)
- **App database:** SQLite for the index, match history, cache and settings
- **Alongside media files:** Kodi / Jellyfin / Emby compatible `.nfo`, posters and fanart

## Fixing bad matches
Mismatches are flagged in the UI. The user picks the right result from the candidates, or searches TMDB or enters an ID manually. Both SQLite and the `.nfo` files update. Each fix is saved as a sample that is used to improve the parsing rules and tests.

## Status
Early development. Contributions and feedback welcome.
