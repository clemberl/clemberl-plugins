# Artist Toolkit

Boîte à skills pour artistes musicaux indépendants, distribuée comme **plugin Claude Code** via la marketplace `clemberl-plugins`.

Les skills couvrent la chaîne de promotion d'une sortie : pitchs streaming, contenu social, coaching créatif, identité visuelle et campagnes de curation Groover.

---

## Installation

Depuis Claude Code :

```
/plugin marketplace add https://github.com/clemberl/clemberl-toolkit
/plugin install artist-toolkit@clemberl-plugins
```

Mise à jour ultérieure :

```
/plugin marketplace update clemberl-plugins
```

---

## Skills inclus

| Skill | Rôle |
|-------|------|
| `streaming-pitch` | Pitchs structurés pour Spotify / Apple Music / Amazon Music for Artists, ancrés dans les paroles et la place du titre dans l'album. |
| `social-content` | Batches de hooks, captions et CTA pour Instagram et TikTok, adaptés au format et à l'objectif. |
| `creative-coach` | Critique de propositions artistiques, test de cohérence avec l'univers, angles de storytelling. Conseille, ne produit pas les livrables finaux. |
| `visual-brief` | Codifie et maintient l'identité visuelle (charte, Brand Kit, briefs visuels ciblés). |
| `curators-groover` | Gestion des campagnes Groover : pitchs généraux et personnalisés, scoring de curateurs, plan de campagne et répartition du budget Grooviz. |

Chaque skill est un fichier `SKILL.md` avec frontmatter YAML. Les skills sont **réutilisables d'un projet musical à l'autre** : ils ne contiennent aucune donnée d'artiste, qui vit dans le projet Claude actif.

---

## Connecteurs

Le plugin est **connector-neutral** : aucun skill n'impose de connecteur en pré-requis gravé. Quand un skill s'appuie sur une source externe, elle est citée comme **exemple nommé** dans la doc, jamais comme dépendance dure.

Le fichier `.mcp.json` à la racine du plugin **propose** les connecteurs dans l'onglet « Connecteurs » sans les imposer :

```json
{
  "mcpServers": {
    "notion": {
      "type": "http",
      "url": "https://mcp.notion.com/mcp"
    }
  }
}
```

- Le serveur Notion hébergé est en HTTP streamable (`type: "http"`) et gère l'OAuth côté serveur : **aucune clé ni token à inscrire**. L'utilisateur s'authentifie au moment où il connecte le serveur depuis l'onglet Connecteurs.
- La clé `"notion"` est le **nom interne** du serveur, référencé par la doc des skills — la garder stable.
- Pour ajouter d'autres connecteurs (Drive, Gmail…), ajouter des entrées sœurs dans `mcpServers`. Un seul `.mcp.json` pour tout le plugin.

---

## Notion & `curators-groover`

Le skill `curators-groover` suppose qu'un **registre de curateurs** et des **templates de pitchs** vivent dans le projet actif (base Notion, fichier joint ou autre). Notion n'est qu'un exemple de support : le skill fonctionne avec n'importe quelle base équivalente.

Pour l'utiliser avec Notion : connecter le serveur `notion` depuis l'onglet Connecteurs, puis pointer le skill vers la base concernée.

---

## Structure du repo

```
.
├── marketplace.json            # référence le plugin via "source": "./artist-toolkit"
└── artist-toolkit/
    ├── .claude-plugin/
    │   └── plugin.json         # nom + version (semver)
    ├── .mcp.json               # connecteurs proposés (Notion, …)
    ├── README.md
    └── skills/
        ├── streaming-pitch/SKILL.md
        ├── social-content/SKILL.md
        ├── creative-coach/SKILL.md
        ├── visual-brief/SKILL.md
        └── curators-groover/SKILL.md
```

---

## Versioning

Le plugin suit **semver en pré-1.0** (`0.x.y`). L'ajout du connecteur Notion via `.mcp.json` est une nouvelle fonctionnalité → **bump mineur** : `0.1.1` → `0.2.0`.

Tenir à jour le champ `version` dans `artist-toolkit/.claude-plugin/plugin.json` à chaque release.

### Changelog

- **0.2.0** — Ajout du `.mcp.json` proposant le connecteur Notion (onglet Connecteurs). Découplage connector-neutral des skills : Notion passe d'un pré-requis gravé à un exemple nommé dans la doc.
- **0.1.1** — Version précédente.
