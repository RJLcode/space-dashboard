# ORBITAL — Space Mission Dashboard

A single-file dashboard for upcoming rocket launches and the International Space Station. No build step, no dependencies. Open `index.html` in a browser or add it to your iPhone home screen as a web app.

## Features

- **Next launch**: live countdown, mission photo, UTC and local T-0, launch window, provider, vehicle and pad. Switch between all launches and US-only. The next three launches are listed below and open in the schedule.
- **Full schedule**: every upcoming launch worldwide (~370), grouped by month.
  - Search by mission, rocket, provider or site.
  - Filter by region (with counts) or show only launches with confirmed dates.
  - Click a row to see its details.
  - Dates are shown only as precisely as they're known, e.g. `NET Q4 2026` or `Time TBD`.
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
| Launches (fallback) | `lldev.thespacedevs.com` dev mirror | No rate limit; data may lag slightly |
| ISS position | [Where the ISS at?](https://wheretheiss.at/w/developer) | Polled every 5 s while the tab is visible |
| Crew | [corquaid/international-space-station-APIs](https://github.com/corquaid/international-space-station-APIs) | Refreshed hourly |

### Rate limiting and caching

A full launch refresh takes about 4 requests (100 launches per page). To stay under the 15-per-hour limit:

- Results are cached in `localStorage` (`orbital.launches.v2`) for **30 minutes** and shared across tabs.
- If the limit is hit partway through a refresh, the pages already fetched are merged with the cached data, and the rest is filled from the dev mirror.
- If nothing is available at all, the dashboard falls back to the dev mirror entirely.

The header badge shows where the data came from:

| Badge | Meaning |
| --- | --- |
| `LIVE` | Fresh data from the production API |
| `CACHED` | Rate-limited, showing saved data |
| `MIRROR` | Data from the dev mirror |
| `OFFLINE` | No launch data available |

Tap the badge (↻) to force a fresh fetch of launches, ISS position and crew. If the production API is rate-limited at that moment, the refresh pulls from the dev mirror instead of keeping the stale cache.

## Stored preferences

The page also saves these in `localStorage`:

- `orbital.scope`: the All/USA choice on the Next Launch panel
- `orbital.region`: the selected schedule region
- `orbital.isstrack`: the recent ISS ground track, so the trail survives reloads

## License

Copyright © 2026 RJLcode (https://github.com/RJLcode). All rights reserved.

This project is **proprietary and not open source**. No part of the source code, data, design, or documentation may be copied, modified, distributed, or used without explicit written permission. See [LICENSE](LICENSE) for the full terms.
