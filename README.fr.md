# PKM

[English](README.md) · **Français**

Wiki d'intégration de la suite *pkm* de plugins [TiddlyWiki](https://tiddlywiki.com) — un système de gestion des connaissances bâti sur des plugins qui font chacun une seule chose et partagent un même schéma :

| Plugin | Dépôt | Rôle |
|---|---|---|
| PKM Schema | [TW-PKM-Schema](https://github.com/nikorion/TW-PKM-Schema) | les champs, leurs vocabulaires, icônes, libellés et traductions, et l'API par laquelle les autres plugins les lisent |
| PKM Fields | [TW-PKM-Fields](https://github.com/nikorion/TW-PKM-Fields) | l'interface : place ces champs dans le modèle d'édition des tiddlers, et dans les colonnes de Dynamic Table |
| Detect Language | [TW-Detect-Language](https://github.com/nikorion/TW-Detect-Language) | outillage de dev : suit la langue du navigateur (en-GB, fr-FR) |

Le wiki charge aussi les plugins compagnons, des plugins autonomes plutôt que des membres de la suite, destinés au même digital garden : [TW-Dynamic-Table](https://github.com/nikorion/TW-Dynamic-Table) (des tableaux éditables qui ignorent tout du schéma, que PKM Fields leur fait connaître par les points d'extension du tableau), TW-Table, TW-Math, TW-Chart, TW-Fonts, TW-Hover-Tilt, TW-Plugin-Info-Tree, TW-Scroll-Layout et TW-Tiny-Bootstrap.

Ce dépôt ne contient aucun plugin : seulement un wiki de dev qui les charge tous ensemble, sur quelques tiddlers de démo, pour voir la suite et y travailler comme un tout. Chaque plugin est développé, documenté et construit dans son propre dépôt.

## Prise en main

Les dépôts des plugins sont placés à côté de celui-ci (`../TW-PKM-Schema`, `../TW-PKM-Fields`, …), chaque plugin relié par un lien symbolique `$TIDDLYWIKI_PLUGIN_PATH/nikorion/<name>` → `<repository>/src/<name>` — le seul moyen pour TiddlyWiki de résoudre les entrées `"nikorion/<name>"` de `wiki/tiddlywiki.info`.

```sh
pnpm install
pnpm dev     # wiki d'intégration + rechargement à chaud ; l'URL (port libre aléatoire) s'affiche au démarrage
pnpm build   # docs/PKM-Wiki.html
```

`pnpm dev` surveille les sources de tous les plugins qu'il charge, suite et compagnons, ainsi que `wiki/tiddlers` : une modification de l'un d'eux est poussée directement dans l'onglet de navigateur déjà ouvert, et une modification de `plugin.info` ou d'un module JS redémarre le serveur. Ne pas recharger l'onglet pour voir une modification : il reviendrait tel que le serveur l'a chargé au démarrage. Arrêter avec deux Ctrl+C. `PKM.code-workspace` ouvre ce dépôt et ceux des plugins ensemble.

## Organisation

| Chemin | Rôle |
|---|---|
| `wiki/tiddlywiki.info` | les plugins de la suite, les langues, le build `html` |
| `wiki/tiddlers/About.tid` | ouvert en premier : ce qu'est ce wiki (un digital garden issu d'une pratique de PKM) |
| `wiki/tiddlers/Playground.tid` | la page d'entrée : les tiddlers de démo dans un tableau, et ce qu'on peut essayer |
| `wiki/tiddlers/demo/` | tiddlers de démo (tag `PKM Demo`), un par type de rôle |
| `wiki/tiddlers/language/<lang>/*.multids` | les chaînes d'About et du Playground, un fichier chacun, via le `detect-language-lingo` de Detect Language |
| `wiki/tiddlers/system/` | config de dev : `$:/config/SyncFilter` (empêche les tiddlers de plugin poussés d'être écrits sur le disque), chemins de fichiers, client HMR |
| `scripts/dev.cjs`, `scripts/dev-hmr.cjs`, `nodemon.json` | le serveur de dev et le rechargement à chaud, comme dans chaque dépôt de plugin mais en surveillant toutes les sources de la suite |

## Ajouter un plugin

Qu'il s'agisse d'un membre de la suite ou d'un compagnon : ajouter son `"nikorion/<name>"` à `wiki/tiddlywiki.info`, son `src/<name>` à `WATCH_DIRS` dans `scripts/dev-hmr.cjs`, son `plugin.info` (et ses modules JS, s'il en a) à `nodemon.json`, ses préfixes de tiddlers à `$:/config/SyncFilter` (tous sauf ses réglages `$:/config/nikorion/<name>/`, qui appartiennent à l'utilisateur), et son dossier à `PKM.code-workspace`.

## Licence

MIT — voir `LICENSE`.
