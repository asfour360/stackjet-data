# stackjet-data

Lounge and card fixes for the **StackJet** iPhone app, published here so they reach everyone without an App Store update.

StackJet downloads [`lounges/corrections.json`](lounges/corrections.json) at most once a day, keeps the last valid copy on the iPhone, and from the next time it starts applies it to every lounge it shows, whether the lounge came from the list built into the app, a table in the app (Air Canada, United, Chase, Capital One, Escape) or the network's own website (Priority Pass, Centurion, Admirals Club, Plaza Premium). From StackJet 1.3 (3). A file that doesn't pass the app's checks is ignored, and the app keeps what it had.

## `lounges/corrections.json`

The tables below only describe the format, and the airport codes in them are placeholders. The file itself is the only thing the app reads, and right now it's empty: no lounge is changed.

| Key | What it does |
|---|---|
| `format`, `version` | Always `"stackjet-lounge-corrections"` and `1`. |
| `updated` | Date of the last change, `yyyy-MM-dd`. |
| `remove` | Lounges to hide, each given as `{ "airport": "<3-letter code>", "provider": "<network>", "name": "<lounge name>" }`, the name as shown in the app (case, dashes and a trailing airport code like "— ABC" don't matter). |
| `change` | Fix fields on one lounge, found the same way. Optional: `newName`, `terminal`, `location`, `hours`, `officialURL`, `description`, `restrictions`, `amenities`, `photoURLs`. |
| `add` | New lounges, in the same format as the app's built-in lounge list. |
| `replaceAirports` | `{ "<3-letter code>": [ … ] }`: replaces every lounge of the networks listed for that airport. |

Rules the app checks: 3-letter uppercase airport codes, a known network name, HTTPS links only, non-empty names.

## `cards/corrections.json`

Right now this file is empty: no card is changed.

StackJet downloads this at most once a day (or straight away with Settings › Check for updates) and applies it right away, on top of the card list built into the app.

| Key | What it does |
|---|---|
| `format`, `version` | Always `"stackjet-card-corrections"` and `1`. |
| `updated` | Date of the last change, `yyyy-MM-dd`. |
| `catalogVersion` | The app's built-in card list version this file was written for (`1.7.70` in StackJet 1.3). An app whose built-in list is newer ignores the whole file, so an old fix never undoes a newer app update. |
| `cards` | Whole card entries, in the same format as the app's built-in card list. One with an existing `id` replaces that card (fee, benefits, earn rates…); a new `id` adds a card. Keep benefit `id`s the same so people's tracked credits carry over. |
| `removeCards` | Card `id`s to take out of the card list. Anyone who already has the card in their wallet keeps it. |
| `programs` | Rewards programs, replaced or added by `id`. |
| `french` | `{ "English text": "French text" }` for new or changed text on Canadian cards. |

Rules the app checks: the right format, every card's rewards programs known, no duplicate ids, valid fees. If the result isn't a valid card list the file is ignored.

The data here is public lounge and card information (lounge names, locations and hours; card fees, benefits and earn rates) from the networks' and issuers' own sites. No app code, user data or keys are kept in this repository.
