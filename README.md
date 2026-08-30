# Arcrave-adblock-data

Ad-block filter list data used by Arcrave (a personal research fork of Brave).

## Background

Brave distributes its filter lists via Chromium's component updater, which requires a `BRAVE_SERVICES_KEY`. This key is issued internally by Brave and is unavailable to personal forks — requests to `go-updater.brave.com` without the key return 403.

This repo's workflow checks the upstream every hour. If changes are detected, it repackages the data using **Brave's own packaging script** and publishes it to a GitHub Release, allowing Arcrave to fetch it over plain HTTPS. The script is used as-is, so the merge logic, list ordering, and scriptlet filtering are identical to the official build.

## Sources

| Source | Purpose |
|---|---|
| [brave/adblock-resources](https://github.com/brave/adblock-resources) | `list_catalog.json` and `resources.json` |
| [brave/adblock-lists-mirror](https://github.com/brave/adblock-lists-mirror) | Brave's public snapshots of third-party filter lists |
| [brave/brave-core-crx-packager](https://github.com/brave/brave-core-crx-packager) | Merge and packaging script (`generateAdBlockRustDataFiles.js`) |

## Artifacts

Assets are published under the release tag `data` (overwritten on each update). The asset name is the Brave component ID:

```
https://github.com/Tingruih/arcrave-adblock-data/releases/latest/download/<component_id>
```

| Asset | Content |
|---|---|
| `gkboaolpopklhgplhaaiboijnklogmbc` | `list_catalog.json` — filter list catalog |
| `mfddibmblmbccpadfndgakiopmmhebop` | `resources.json` — scriptlet resource library |
| Other IDs | Merged `list.txt` for the respective filter list |
| `manifest.json` | Upstream commit SHAs and timestamps for reproducibility |

To reproduce a build, use the `mirror_sha` from `manifest.json`:

```
npm run data-files-ad-block-rust -- --commit-hash <mirror_sha>
```

## License & Attribution

This repo does **not** own or modify any filter rules; it simply repackages them for a personal research fork.

Copyright and license terms for each filter list belong to their respective authors — EasyList, EasyPrivacy, uBlock Origin filters, AdGuard lists, etc., licensed under CC BY-SA 3.0, GPLv3, and others. See the `sources[].url` field in [`list_catalog.json`](https://github.com/brave/adblock-resources/blob/master/filter_lists/list_catalog.json) for the full source list.

`resources.json` is derived from [uBlock Origin](https://github.com/gorhill/uBlock) scriptlets (GPLv3).

The packaging script is from brave-core-crx-packager (MPL 2.0).

This repo is not affiliated with or endorsed by Brave Software.
