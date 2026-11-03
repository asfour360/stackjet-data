# stackjet-data

Lounge fixes for the **StackJet** iPhone app, published here so they reach everyone without an App Store update.

StackJet downloads [`lounges/corrections.json`](lounges/corrections.json) at most once a day, keeps the last valid copy on the iPhone, and applies it on top of the lounges built into the app the next time it starts. A file that doesn't pass the app's checks is ignored, and the app keeps what it had.

## `lounges/corrections.json`

| Key | What it does |
|---|---|
| `format`, `version` | Always `"stackjet-lounge-corrections"` and `1`. |
| `updated` | Date of the last change, `yyyy-MM-dd`. |
| `remove` | Lounges to hide: `{ "airport": "YVR", "provider": "Priority Pass", "name": "…" }` (exact name as in the app). |
| `change` | Fix fields on one lounge, found the same way. Optional: `newName`, `terminal`, `location`, `hours`, `officialURL`, `description`, `restrictions`, `amenities`, `photoURLs`. |
| `add` | New lounges, in the same format as the app's built-in lounge list. |
| `replaceAirports` | `{ "YVR": [ … ] }`: replaces every lounge of the networks listed for that airport. |

Rules the app checks: 3-letter uppercase airport codes, a known network name, HTTPS links only, non-empty names.

The data here is public lounge information (names, locations, hours) from the networks' own sites. No app code, user data or keys are kept in this repository.
