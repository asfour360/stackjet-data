# stackjet-data

Lounge, card and other data fixes for the **StackJet** iPhone app, published here so they reach everyone without an App Store update.

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

## `app/data.json`

Right now this file is empty: nothing is changed. Fixes for StackJet's smaller tables (from StackJet 1.3 (3)). The app downloads it at most once a day (or with Settings › Check for updates) and uses it from the next time it starts. Every section is optional and stands alone: a missing or empty section, or one with a bad entry, leaves that table exactly as built into the app.

| Section | What it does |
|---|---|
| `transferPartners` | `{ "<source program id>": [ { "id", "name", "category", "from", "to", "unit", "programID"? } ] }`. The Points page shows exactly this partner list for each source program listed (e.g. `amex-mr-can`). `programID` is only needed for a partner id the app doesn't know yet. |
| `storeChains` | `[ { "match": ["<store name>", …], "category": "<card category>" } ]`, checked before the built-in chains (e.g. `loblawsGroup`, `groceries`, `gas`). |
| `princeOfTravelSources`, `princeOfTravelPartners` | Prince of Travel's program codes -> the app's programs and transfer partners, added to the built-in names. |
| `pointsGuySources`, `frequentMilerSources`, `partnerNames` | Names The Points Guy and Frequent Miler use -> the app's programs and partners, checked first. |
| `statusMatchRules` | When present, the whole list of status match offers (`source`, `minRank`, `destProgram`, `destRank`, `dest`, `instant`, `detail`, `countries`?). |
| `statusMatchCards` | `[ { "name", "aliases"?, "grants": [ { "program", "rank", "tier", "note"? } ], "mqd"?, "country"?, "note"? } ]`: a card on the Status matches page and the statuses it gives. Replaces the built-in card with the same name, or adds it. |
| `statusProgramNames` | `{ "<status match program id>": "<name>" }`: the name shown on the Status matches page (e.g. `"bw": "WorldHotels Rewards"` after a merger). |
| `pointValueNames` | `[ { "name": "<name on the point values page>", "ids": ["<program id>"] } ]`: extra names to read, for a program renamed on that page. |
| `statusTiers` | `{ "<program id>": ["<lowest tier>", …] }`: the tier names for each program listed. |
| `retiredCards` | `{ "<closed card id>": "<replacement card id>" }`. Used only when the replacement card is in the app's card list. |
| `homeCities` | Points deals departure cities `{ "city", "airports" }`: replaces the city with the same name, or adds it. |
| `nearbyCities` | `{ "<city>": ["<nearby city>", …] }` for each city listed. |
| `bestUses` | When present, the whole "Best uses of your points" list (same fields as the app's built-in list: `id`, `programID`, `partnerID`?, `programName`, `kind` `flight`/`hotel`, `cabin` for flights, `points` **or** `rule`+`ruleFR`, `title`/`titleFR`, `unit`/`unitFR`, `why`/`whyFR`, `tip`/`tipFR`, `sourceName`, `sourceURL`, `checked`). One bad entry and the built-in list is used. |
| `sources` | Websites the app reads, by key: `princeOfTravel`, `pointsGuyBonuses`, `frequentMiler`, `flytrippers`, `flyingBluePromos`, `pointValues`. `{ "enabled": false }` stops reading one (its old results go away; point values keep the last ones); `{ "url": "https://…" }` reads it at a new address (same page layout). |
| `liveLounges` | `{ "<network>": false }` (e.g. `"Priority Pass"`): stop reading that network's own website and show the app's built-in list for it, plus the lounge fixes. |

## `status/benefits.json`

What each status tier gives, shown when someone taps a status on StackJet's Status page (from StackJet 1.3 (3)). The app ships a copy and uses this file when it's newer (by `updated`), checked at most once a day. Changes show straight away.

| Key | What it does |
|---|---|
| `format`, `version`, `updated` | `"stackjet-status-benefits"`, `1`, and the date of the last change. Bump `updated` with every change. |
| `programs` | `{ "<program id>": { "sourceName", "sourceURL" (HTTPS), "checked", "note"?, "noteFR"?, "tiers": [ … ] } }`, keyed by the app's program id (`delta`, `hilton`…). |
| `tiers` | Lowest first: `{ "name", "nameFR"?, "qualify"?, "qualifyFR"?, "benefits": [ … ], "benefitsFR": [ … ] }`. Use the app's tier names (e.g. `Hilton Honors Gold`) so the person's tier is marked. `benefitsFR` must have one line per English line. |

The data here is public lounge and card information (lounge names, locations and hours; card fees, benefits and earn rates) from the networks' and issuers' own sites. No app code, user data or keys are kept in this repository.
