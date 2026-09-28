# KMS — contexte projet pour Claude

> **Avant toute tâche ici, consulter d'abord le `CLAUDE.md` du workspace** (`../CLAUDE.md`, dont § Suite kms) et ses `guides/` (HMR, pièges PowerShell/Windows, `publishFilter`). Ci-dessous : uniquement le spécifique à KMS.

## Ce que c'est
Wiki de dev d'**intégration** de la suite kms (créé le 2026-09-28) : charge ensemble `nikorion/kms-ontology`, `base-fields`, `dyntable`, `detect-language` sur des tiddlers de démo (`wiki/tiddlers/demo/`, tag `KMS Demo`) et un Playground. **Aucun plugin ici** : chaque plugin se développe, se documente et se builde dans son propre dépôt ; ne jamais y déposer de source de plugin. `pnpm build` ne produit que `docs/KMS-Wiki.html` (pas de target `plugin-json`).

`README.md` = développeur (installation, disposition, ajout d'un plugin à la suite) ; ce fichier = modèle ; pas de readme de plugin (pas de plugin).

## Spécificités
- **Le HMR surveille les sources de tous les plugins de la suite** (`WATCH_DIRS` de `scripts/dev-hmr.cjs` : `../TW-KMS-Ontology/src/kms-ontology`, `../TW-Base-Fields/src/base-fields`, `../TW-Dynamic-Table/src/dyntable`, `../TW-Detect-Language/src/detect-language`, `wiki/tiddlers`) ; `nodemon.json` redémarre sur leurs `plugin.info` et les modules JS de detect-language. C'est donc **le** wiki où vérifier une modif qui traverse plusieurs plugins (ontologie → éditeur → tableaux) : les wikis de dev des plugins ne poussent que leurs propres sources.
- **Avant d'ajouter un plugin à la suite → lire `README.md` § Adding a plugin to the suite** (5 endroits : `tiddlywiki.info`, `WATCH_DIRS`, `nodemon.json`, `$:/config/SyncFilter`, `KMS.code-workspace`) ; symlink `TIDDLYWIKI_PLUGIN_PATH` requis (voir `../CLAUDE.md` § Chargement des plugins).
- **`$:/config/SyncFilter` exclut les préfixes de tous les plugins poussés à chaud** (et leurs shadows hors namespace : `$:/config/EditTemplateFields/Visibility/`, `$:/tags/nikorion/kms/Field`, `$:/config/nikorion/base-fields/`, `$:/config/nikorion/kms-ontology/`) : sinon le syncer écrit les overrides HMR en vrais tiddlers sous `wiki/tiddlers/system/`, qui masquent ensuite les plugins. `$:/config/nikorion/dyntable/` n'est **pas** exclu : ce sont des réglages utilisateur.
- **Ne jamais conseiller un rechargement d'onglet pour voir une modif** (il annule le HMR).
- Contenu du wiki (Playground, démo) : prose anglaise, i18n via `detect-language-lingo` (`wiki/tiddlers/language/<lang>/playground.multids`), comme les autres wikis de dev.
