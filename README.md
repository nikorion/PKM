# KMS

Integration wiki of the *kms* suite of [TiddlyWiki](https://tiddlywiki.com) plugins — a knowledge management system built out of plugins that each do one thing and share one ontology:

| Plugin | Repository | Role |
|---|---|---|
| KMS Ontology | [TW-KMS-Ontology](https://github.com/nikorion/TW-KMS-Ontology) | the fields, their vocabularies, icons, labels and translations, and the API the other plugins read them through |
| Base Fields | [TW-Base-Fields](https://github.com/nikorion/TW-Base-Fields) | the editor: puts those fields in the tiddler edit template |
| Dynamic Table | [TW-Dynamic-Table](https://github.com/nikorion/TW-Dynamic-Table) | editable tables, with a column for each field |
| Detect Language | [TW-Detect-Language](https://github.com/nikorion/TW-Detect-Language) | dev tooling: follows the browser's language (en-GB, fr-FR) |

This repository holds no plugin: only a dev wiki that loads them all together, on a few demo tiddlers, to see and work on the suite as a whole. Each plugin is developed, documented and built in its own repository.

## Getting started

The plugin repositories sit next to this one (`../TW-KMS-Ontology`, `../TW-Base-Fields`, …), each plugin symlinked as `$TIDDLYWIKI_PLUGIN_PATH/nikorion/<name>` → `<repository>/src/<name>` — the only way TiddlyWiki resolves the `"nikorion/<name>"` entries of `wiki/tiddlywiki.info`.

```sh
pnpm install
pnpm dev     # integration wiki + hot reload; the URL (random free port) is printed on start
pnpm build   # docs/KMS-Wiki.html
```

`pnpm dev` watches the sources of every plugin of the suite as well as `wiki/tiddlers`: an edit to any of them is pushed straight into the browser tab already open, and a `plugin.info` or JS module change restarts the server. Do not reload the tab to see a change: it would come back as the server loaded it at boot. Stop with Ctrl+C twice. `KMS.code-workspace` opens this repository and the plugin repositories together.

## Layout

| Path | Role |
|---|---|
| `wiki/tiddlywiki.info` | the plugins of the suite, the languages, the `html` build |
| `wiki/tiddlers/Playground.tid` | the entry page: the demo tiddlers in a table, and what to try |
| `wiki/tiddlers/demo/` | demo tiddlers (tag `KMS Demo`), one per kind of role |
| `wiki/tiddlers/language/<lang>/playground.multids` | the Playground's strings, through Detect Language's `detect-language-lingo` |
| `wiki/tiddlers/system/` | dev config: `$:/config/SyncFilter` (keeps pushed plugin tiddlers out of the disk), file paths, HMR client |
| `scripts/dev.cjs`, `scripts/dev-hmr.cjs`, `nodemon.json` | the dev server and hot reload, as in every plugin repository but watching all the suite's sources |

## Adding a plugin to the suite

Add its `"nikorion/<name>"` to `wiki/tiddlywiki.info`, its `src/<name>` to `WATCH_DIRS` in `scripts/dev-hmr.cjs`, its `plugin.info` (and JS modules, if any) to `nodemon.json`, its tiddler prefixes to `$:/config/SyncFilter`, and its folder to `KMS.code-workspace`.

## License

MIT — see `LICENSE`.
