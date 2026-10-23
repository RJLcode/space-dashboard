# ORBITAL — Space Mission Dashboard

A single-file dashboard for upcoming and recent rocket launches and the International Space Station. No build step, no dependencies. Open `index.html` in a browser or add it to your iPhone home screen as a web app.

## Features

- **Next launch**: live countdown, mission photo, UTC and local T-0, launch window, provider, vehicle and pad. Shows US launches by default; switch to all launches anytime. The next three launches are listed below and open in the schedule.
- **Full schedule**: every upcoming launch worldwide (~370), grouped by month.
  - Search by mission, rocket, provider or site.
  - Filter by region (with counts, USA by default) or show only launches with confirmed dates.
  - Click a row to see its details.
  - Dates are shown only as precisely as they're known, e.g. `NET Q4 2026` or `Time TBD`.
- **Recent launches**: an Upcoming / Recent switch on the schedule shows the last 30 launches, newest first, with how long ago each flew and its result (`SUCCESS`, `FAILURE`, `PARTIAL`, or `PENDING` until the API records an outcome). Search and region filters work here too. The page always opens on Upcoming.
- **ISS**: live position on a world map with about one orbit of ground track, plus latitude, longitude, altitude and velocity, updated every 5 seconds.
- **People in space**: current crews grouped by station, with agency, role and days in orbit.
- **Responsive**: two-column layout on desktop, stacked on tablet, and bottom tabs on phones.

## Running locally

Opening the file directly works:

```bash
open index.html
```

Or serve it on `http://localhost:8765`:

```bash
python3 -m http.server 8765
```

`.claude/launch.json` holds the same server config for the Claude Code preview pane.

## Data sources

| Data | Source | Notes |
| --- | --- | --- |
| Launches | [Launch Library 2](https://thespacedevs.com/llapi) (`ll.thespacedevs.com/2.2.0`, list mode) | Free tier: 15 requests/hour per IP |
| Recent launches | Launch Library 2 `/launch/previous/` (last 30) | Fetched only when Recent is opened |
| Launches (fallback) | `lldev.thespacedevs.com` dev mirror | No rate limit, but incomplete; used only with no saved data |
| ISS position | [Where the ISS at?](https://wheretheiss.at/w/developer) | Polled every 5 s while the tab is visible |
| Crew | [corquaid/international-space-station-APIs](https://github.com/corquaid/international-space-station-APIs) | Refreshed hourly |

### Rate limiting and caching

The 15-per-hour limit is per network (IP address), so every device on the same Wi-Fi shares it. To stay under it:

- The first page (the next 100 launches) is refreshed every **30 minutes**: one request.
- The rest of the list (about 4 more pages) changes rarely and is refreshed every **6 hours**; in between it's reused from the cache.
- Results are cached in `localStorage` (`orbital.launches.v2`) and shared across tabs. Each browser and home-screen app has its own cache.
- If the limit is hit partway through a refresh, the pages already fetched are merged with the cached list.
- If production is rate-limited, saved data is shown (`CACHED`). The dashboard asks the API's `api-throttle` endpoint (free to call) when the quota frees up and retries production at that moment.
- The dev mirror is only used when there's no saved data at all: it isn't rate limited, but it's missing many launches.
- Recent launches cost one extra request, made only the first time the Recent view is opened. They're cached separately (`orbital.past.v1`) for **2 hours** and fall back to the dev mirror if rate-limited.

The header badge shows where the data came from:

| Badge | Meaning |
| --- | --- |
| `LIVE` | Fresh data from the production API |
| `CACHED` | Rate-limited, showing saved data |
| `MIRROR` | Data from the dev mirror |
| `OFFLINE` | No launch data available |

Tap the badge (↻) to force a fresh fetch of launches (one request), ISS position, crew and, when open, Recent launches.

## Stored preferences

The page also saves these in `localStorage`:

- `orbital.isstrack`: the recent ISS ground track, so the trail survives reloads

The All/USA, region and Upcoming/Recent choices are deliberately not saved, so every visit starts on **USA** for Next Launch, Upcoming and Recent, and on the Upcoming view. Switch to All or another country at any time.

## License

Copyright © 2026 RJLcode (https://github.com/RJLcode). All rights reserved.

This project is **proprietary and not open source**. No part of the source code, data, design, or documentation may be copied, modified, distributed, or used without explicit written permission. See [LICENSE](LICENSE) for the full terms.
