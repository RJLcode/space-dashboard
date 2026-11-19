# Changelog

All notable changes to ORBITAL are documented here.

This project follows [Semantic Versioning](https://semver.org/): `vMAJOR.MINOR.PATCH`

| Part | When to bump | Example |
|------|-------------|---------|
| **MAJOR** | Complete UI overhaul | `v1.0.0` → `v2.0.0` |
| **MINOR** | New panel, data source, or feature | `v1.0.0` → `v1.1.0` |
| **PATCH** | Bug fix, label tweak, or cosmetic change | `v1.0.0` → `v1.0.1` |

---

## [v1.0.1] — 2026-11-19

- Add to Home Screen: iPhone now gets a real app icon. The old icon was an SVG, which iOS ignores, so it fell back to a screenshot. Added a PNG icon set and a web app manifest
- Every request now has a time limit (8 seconds for the ISS, 12 for crew and the rate-limit check, 20 for launch pages), so a hung connection shows its error instead of waiting forever
- ISS panel says "ISS position unavailable" after three missed updates in a row and clears itself when the position comes back
- Crew list: a failed refresh keeps the last list on screen and retries in 5 minutes; the hourly refresh skips hidden tabs and catches up when the page is back on screen
- Dim labels are a little brighter so they meet WCAG AA contrast on every panel; focused controls show an outline; animations are off with Reduce Motion
- Added `.gitignore`, `CHANGELOG.md` and `CLAUDE.md`, and a version number

## [v1.0.0] — 2026-10-22

- Baseline: next launch countdown, full and recent launch schedule with search and region filters, live ISS map, and people in space
