# PKM — contexte projet pour Claude

> **Avant toute tâche ici, consulter d'abord le `CLAUDE.md` du workspace** (`../CLAUDE.md`, dont § Suite pkm) et ses `guides/` (HMR, pièges PowerShell/Windows, `publishFilter`). Ci-dessous : uniquement le spécifique à PKM.

## Ce que c'est
**pkm-dev** : wiki de dev d'**intégration** de la suite pkm (créé le 2026-09-28) : charge la suite (`nikorion/pkm-schema`, `pkm-fields`, `detect-language`) et tous les compagnons (`dyntable`, `table`, `math`, `chart`, `fonts`, `hover-tilt`, `plugin-info-tree`, `scroll-layout`, `tiny-bootstrap`) sur des tiddlers de démo (`wiki/tiddlers/demo/`, tag `PKM Demo`) et un Playground. **Aucun plugin ici** : chaque plugin se développe, se documente et se builde dans son propre dépôt ; ne jamais y déposer de source de plugin. `pnpm build` ne produit que `docs/PKM-Wiki.html` (pas de target `plugin-json`).

`README.md` = développeur (installation, disposition, ajout d'un plugin à la suite) ; ce fichier = modèle ; pas de readme de plugin (pas de plugin).

## Spécificités
- **Le HMR surveille les sources de tous les plugins chargés**, suite et compagnons (déduites par `../tw-dev` des plugins de `tiddlywiki.info`, + `wiki/tiddlers`) ; nodemon redémarre sur leurs `plugin.info` et modules JS. C'est donc **le** wiki où vérifier une modif qui traverse plusieurs plugins (schéma → éditeur → tableaux) : le wiki de dev d'un plugin ne pousse que les plugins qu'il charge.
- **Avant d'ajouter un plugin à pkm-dev (suite ou compagnon) → lire `README.md` § Adding a plugin** (2 endroits : `tiddlywiki.info`, `PKM.code-workspace` ; sources surveillées et `$:/config/SyncFilter` couvrent déjà tout plugin nikorion) ; symlink `TIDDLYWIKI_PLUGIN_PATH` requis (voir `../CLAUDE.md` § Chargement des plugins).
- **`$:/config/SyncFilter` exclut les préfixes de tous les plugins poussés à chaud** (et leurs shadows hors namespace : `$:/config/EditTemplateFields/Visibility/`, `$:/tags/nikorion/pkm/Field`, `$:/config/nikorion/pkm-fields/`, `$:/config/nikorion/pkm-schema/`, `$:/fonts/` de fonts, `$:/core/ui/PluginInfo/Default/contents` de plugin-info-tree) : sinon le syncer écrit les overrides HMR en vrais tiddlers sous `wiki/tiddlers/system/`, qui masquent ensuite les plugins. Les `$:/config/nikorion/<compagnon>/` (dyntable, math, hover-tilt, scroll-layout) ne sont **pas** exclus : ce sont des réglages utilisateur.
- **Ne jamais conseiller un rechargement d'onglet pour voir une modif** (il annule le HMR).
- Contenu du wiki (Playground, démo) : prose anglaise, i18n via `detect-language-lingo` (`wiki/tiddlers/language/<lang>/playground.multids`), comme les autres wikis de dev.
