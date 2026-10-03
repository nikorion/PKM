# PKM

**English** · [Français](README.fr.md)

Integration wiki of the *pkm* suite of [TiddlyWiki](https://tiddlywiki.com) plugins — a knowledge management system built out of plugins that each do one thing and share one schema:

| Plugin | Repository | Role |
|---|---|---|
| PKM Schema | [TW-PKM-Schema](https://github.com/nikorion/TW-PKM-Schema) | the fields, their vocabularies, icons, labels and translations, and the API the other plugins read them through |
| PKM Fields | [TW-PKM-Fields](https://github.com/nikorion/TW-PKM-Fields) | the interface: puts those fields in the tiddler edit template, and in Dynamic Table's columns |
| Detect Language | [TW-Detect-Language](https://github.com/nikorion/TW-Detect-Language) | dev tooling: follows the browser's language (en-GB, fr-FR) |

The wiki also loads the companion plugins, standalone plugins rather than members of the suite, meant for the same digital garden: [TW-Dynamic-Table](https://github.com/nikorion/TW-Dynamic-Table) (editable tables that know nothing of the schema, which PKM Fields teaches through the table's extension points), TW-Table, TW-Math, TW-Chart, TW-Fonts, TW-Hover-Tilt, TW-Plugin-Info-Tree, TW-Scroll-Layout and TW-Tiny-Bootstrap.

This repository holds no plugin: only a dev wiki that loads them all together, on a few demo tiddlers, to see and work on the suite as a whole. Each plugin is developed, documented and built in its own repository.

## Contents

- [Getting started](#getting-started)
- [Layout](#layout)
- [Adding a plugin](#adding-a-plugin)
- [License](#license)

## Getting started

The plugin repositories sit next to this one (`../TW-PKM-Schema`, `../TW-PKM-Fields`, …), each plugin symlinked as `$TIDDLYWIKI_PLUGIN_PATH/nikorion/<name>` → `<repository>/src/<name>` — the only way TiddlyWiki resolves the `"nikorion/<name>"` entries of `wiki/tiddlywiki.info`.

```sh
pnpm install
pnpm dev     # integration wiki + hot reload; the URL (random free port) is printed on start
pnpm build   # docs/PKM-Wiki.html
```

`pnpm dev` watches the sources of every plugin it loads, suite and companions, as well as `wiki/tiddlers`: an edit to any of them is pushed straight into the browser tab already open, and a `plugin.info` or JS module change restarts the server. Do not reload the tab to see a change: it would come back as the server loaded it at boot. Stop with Ctrl+C twice. `PKM.code-workspace` opens this repository and the plugin repositories together.

[↑](#contents "Back to contents")

## Layout

| Path | Role |
|---|---|
| `wiki/tiddlywiki.info` | the plugins of the suite, the languages, the `html` build |
| `wiki/tiddlers/About.tid` | opened first: what this wiki is (a digital garden grown out of a PKM practice) |
| `wiki/tiddlers/Playground.tid` | the entry page: the demo tiddlers in a table, and what to try |
| `wiki/tiddlers/demo/` | demo tiddlers (tag `PKM Demo`), one per kind of role |
| `wiki/tiddlers/language/<lang>/*.multids` | the strings of About and the Playground, one file each, through Detect Language's `detect-language-lingo` |
| `wiki/tiddlers/system/` | dev config: `$:/config/SyncFilter` (keeps pushed plugin tiddlers out of the disk), file paths, HMR client |
| `scripts/dev.cjs`, `scripts/dev-hmr.cjs`, `nodemon.json` | the dev server and hot reload, as in every plugin repository but watching all the suite's sources |

[↑](#contents "Back to contents")

## Adding a plugin

A member of the suite or a companion alike: add its `"nikorion/<name>"` to `wiki/tiddlywiki.info`, its `src/<name>` to `WATCH_DIRS` in `scripts/dev-hmr.cjs`, its `plugin.info` (and JS modules, if any) to `nodemon.json`, its tiddler prefixes to `$:/config/SyncFilter` (all but its `$:/config/nikorion/<name>/` settings, which are the user's), and its folder to `PKM.code-workspace`.

[↑](#contents "Back to contents")

## License

MIT — see `LICENSE`.

[↑](#contents "Back to contents")
