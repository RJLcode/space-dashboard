# Claude Code Instructions

- Always bump the patch version in index.html (the `<!-- ORBITAL vX.Y.Z | YYYY-MM-DD -->` comment at the top), README.md, and CHANGELOG.md when making changes
- Add a dated entry to CHANGELOG.md for every index.html change
- Update README.md's Features, Data sources or Stored preferences sections if a change affects them
- Launch Library allows 15 requests/hour per network: don't add requests without checking the caching rules in README.md
- Keep it a single `index.html` with vanilla JavaScript: no frameworks, build steps or dependencies
- Escape feed data with `esc()` before putting it into HTML; use `tfetch()` (fetch with a timeout) for network requests
- .gitignore is present — do not commit node_modules or other ignored files
- Version format is `vMAJOR.MINOR.PATCH` — bug fixes and cosmetic changes are PATCH only
