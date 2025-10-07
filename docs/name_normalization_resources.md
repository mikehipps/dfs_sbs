# Name Normalization Resources

This document summarizes options for augmenting the handcrafted `nicknames_common.json` and `suffixes.json` datasets.

## Nicknames

| Source | Notes |
| --- | --- |
| [`joshfraser/nicknames`](https://github.com/joshfraser/nicknames) | MIT-licensed JSON and CSV nickname mappings covering hundreds of common English diminutives. Easy to import as a drop-in replacement or merge with existing mappings. |
| [`censusname/nickname_lookup`](https://github.com/censusname/nickname_lookup) | Includes SSA nickname crosswalk with frequency metadata. Could allow weighting, but requires light parsing to extract mappings. |
| [`openvenues/libpostal`](https://github.com/openvenues/libpostal/tree/master/resources/dictionaries) | Contains broader lexical dictionaries, including nickname-like expansions, but bundled with other locale data. Useful if we want international coverage but heavier dependency. |
| [`probablepeople`](https://github.com/datamade/probablepeople) | Python library for parsing names that ships with curated nickname tables inside its data files. Can be vendored if we prefer Python data structures over raw JSON. |

These sources can be vendored or referenced at build time to pre-generate a richer nickname table. Because they are open-source and versioned, we can periodically re-run an ingestion script instead of manually appending entries.

## Suffixes / Name Affixes

| Source | Notes |
| --- | --- |
| [`probablepeople`](https://github.com/datamade/probablepeople/blob/master/probablepeople/data/constants.py) | Maintains extensive enumerations of suffixes (e.g., *Jr.*, *Sr.*, *III*), prefixes, and other honorifics. Licensed under the GPLv3, so we would need to vendor only the data or keep a compatible license. |
| [`nameparser`](https://github.com/derek73/python-nameparser/blob/master/src/nameparser/config/prefixes.py) | BSD-licensed dataset of honorifics and suffixes. The suffix list is more comprehensive than our current JSON and easy to serialize. |
| [`usaddress`](https://github.com/datamade/usaddress)`'s` lexical data | Contains numerous personal-name tokens, including generational suffixes, though not as directly consumable. |

For suffixes, importing a vetted list from `nameparser` or `probablepeople` would eliminate manual upkeep.

## Recommendation

Adopting one of the open datasets (for example, vendoring the `joshfraser/nicknames` JSON and the `nameparser` suffix list) gives us comprehensive coverage with minimal ongoing maintenance. We can still keep local overrides for sports-specific spellings, but versioning the upstream data keeps updates straightforward. Until we wire up an ingestion step, it is reasonable to append entries as we encounter them—just document the source so that we can reconcile with the upstream dataset later.
