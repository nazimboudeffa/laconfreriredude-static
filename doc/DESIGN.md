# Design — La Confrérie du Dé (site statique)

Documentation du design appliqué à `index.html`, dérivé de la charte graphique
(`doc/01_Graphic Charter.pdf`). Ce document décrit les choix de ton, de
composition et d'accessibilité mis en place, et sert de référence pour toute
évolution.

## 1. Principe général

Le site reprend l'identité « affiche » de la charte : un système noir & blanc
très contrasté, rehaussé d'orange, de rouge et de violet. La lecture alterne
des sections **sur fond noir** (nav, hero, événements, CTA, footer) et des
sections **sur fond blanc** (soirées, agenda, JDR) pour rythmer le parcours.

Chaque couleur de la charte s'exprime à deux niveaux :

- en **accent décoratif** (tailles de texte importantes, pictogrammes, formes) ;
- en **support de lecture** (texte, liens, boutons), dans des variantes
  calculées pour garantir un bon contraste (WCAG AA).

## 2. Palette et tokens

Les couleurs sont déclarées dans `:root` de `index.html`, sous forme de
variables CSS (seul endroit à modifier si la palette évolue).

| Variable          | Hex / valeur      | Usage                                                        |
| ----------------- | ----------------- | ------------------------------------------------------------ |
| `--black`         | `#000000`         | Fonds des sections sombres, nav, footer.                     |
| `--white`         | `#FFFFFF`         | Fonds clairs, texte de niveau 1 sur fonds sombres.           |
| `--orange`        | `#F7964B`         | Charte — accent sur fonds sombres (eyebrow, dates, focus).   |
| `--red`           | `#EF423B`         | Charte — rouge décoratif, grandes tailles (cœur du footer).  |
| `--red-bright`    | `#FF7068`         | Rouge clair, texte rouge lisible sur noir (urgences).        |
| `--red-mid`       | `#C92B25`         | Rouge accessible (5,45:1 sur blanc) : boutons, tags, lignes. |
| `--red-deep`      | `#B01F1A`         | Rouge accessible renforcé (6,87:1 sur blanc) : liens, survol.|
| `--purple`        | `#591F60`         | Charte — violet, lisible sur blanc (badges, bordures).       |
| `--purple-soft`   | `rgba(89,31,96,.09)` | Fond des badges / hover violet clair.                    |
| `--ink`           | `#101013`         | Texte principal sur fond blanc (meilleure lisibilité).       |
| `--ink-soft`      | `#3F3F46`         | Texte secondaire sur fond blanc.                             |
| `--ink-third`     | `#6B6B72`         | Texte tertiaire / lieux / notes.                             |
| `--white-80/70`   | blanc translucide | Texte secondaire/tertiaire sur fonds sombres.                |
| `--line-soft`     | `rgba(16,16,19,.14)` | Séparateurs sur fond blanc.                              |
| `--white-line(-soft)` | blanc translucide | Bordures sur fonds sombres.                            |

> Note accessibilité : le rouge charte `#EF423B` (3,3:1 sur blanc) n'est pas
> assez contrasté pour du petit texte. Il sert uniquement en décor de grande
> taille ; les textes rouges utilisent `--red-mid` / `--red-deep` / `--red-bright`.

## 3. Typographie

Deux familles, auto-hébergées dans `fonts/` via `@font-face`
(`font-display: swap`).

| Rôle    | Police          | Graisses chargées                              | Application                        |
| ------- | --------------- | ---------------------------------------------- | ---------------------------------- |
| Titrage | **Auster**      | Regular, SemiBold, Bold, Black (`fonts/Auster/`) | h1–h4, nav, dates événements.    |
| Texte   | **Acumin Pro**  | Regular, Semibold, Bold (`fonts/Acumin/`)        | corps, boutons, lisibilité.       |

Règles d'usage :

- Les titres utilisent Auster en graisse **900** (ou 700 pour les cartes) et un
  interlignage resserré (`line-height: 1.12`).
- Le corps de texte est en Acumin Pro, `line-height: 1.6`, avec des variantes
  `--ink-soft` / `--ink-third` pour hiérarchiser.
- Chaque police déclare une pile de repli (`Trebuchet MS`, `Segoe UI`,
  `Tahoma`) pour fonctionner hors-ligne ou si les fichiers manquent.
- La tagline « It's Play Time ! » de la charte utiliserait une police
  Timberline **non fournie** : elle n'est pas utilisée sur le site.

## 4. Logotype

- Source : `doc/La Confrerie du De_Logotype_RVB/`, déclinaison
  **White_Round** (dé blanc à points rouges).
- Copie d'exploitation : `images/logo_confrerie_blanc.png`.
- Usages : mark de la nav (30×30) et visuel animé du hero. Sur les fonds clairs,
  seule la variante **noire** du logo doit être utilisée ; la variante blanche
  est réservée aux fonds sombres.
- Le logo des partenaires est fourni en `images/` (logos clampés dans des
  cadres blancs de 96px / 108px).

## 5. Composition

- Conteneur : `--max-w` 1080px, espacement latéral 28px, sections
  `padding: 84px 0`.
- Chaque section commence par un **en-tête structuré** : `section-tag`
  (sur-ligne), `h2` très gras avec une **barre de 3px sous le titre**
  (rouge sur fond clair, orange sur fond sombre), puis un paragraphe
  d'introduction.
- Rythme clair/sombre :
  - **Sombre** (noir) : nav, hero, « Événements », CTA, footer.
  - **Clair** (blanc) : « Soirées », « Agenda », « Jeu de rôle ».
- Les tags et accents passent au **orange** sur fond sombre, au
  **rouge accessible** sur fond clair.
- Point de rupture : **860px** (colonnes empilées en 1 colonne, dé caché en
  mobile, nav en colonne).

## 6. Composants clés

| Composant        | Description                                                               |
| ---------------- | ------------------------------------------------------------------------- |
| Nav              | Sticky, fond noir, mark logo + mot, liens en pilules bordées.             |
| Hero             | Fond noir, motif de points, halos orange/violet, titre 900, dé à lancer  |
|                  | (animation `dieRoll` 8,5 s, masquée si « reduce-motion »).               |
| Boutons          | Primaire rouge `--red-mid` (hover `--red-deep`), ghost sur fond sombre,  |
|                  | outline violet sur fond clair (hover élévation).                          |
| Cartes de salles | Tuile blanche, bord supérieur 4px coloré par **statut** :                |
|                  | actif = `--red-mid`, nouveau = `--purple`, pause = `--ink-third`.        |
| Agenda           | Deux colonnes (Dream Team / Bowling) avec points `--red-mid` / `--purple`,|
|                  | horaires en `tabular-nums`.                                               |
| Callout          | Panneau noir « On cherche des volontaires », puces `◆` orange.           |
| Timeline         | Fil noir sur fond sombre, pastilles orange, puce rouge si **urgent**;    |
|                  | badge d'urgence rouge.                                                    |
| Spotlight        | Carte événement photo/copy, zoom photo au survol (respecte reduce-motion).|
| JDR              | Colonne texte + carte violette en pointillés (`border dashed`).          |
| CTA / footer     | Fonds noirs, chips en pilules, icônes sociales colorées (Facebook/Discord).|

## 7. Accessibilité

- **Contrastes** : toutes les associations texte/couleur visent le niveau AA.
  Rouge charte réservé au décor (voir § 2). Noir `#000`/blanc `#FFF` pour le
  principal, `#101013` sur blanc pour le corps.
- **Focus visible** : `:focus-visible` = contour orange 2px + offset 2px,
  appliqué globalement.
- **Mouvement** : `prefers-reduced-motion: reduce` coupe le défilement fluide
  et toutes les animations/transitions.
- **Liens externes** : `target="_blank" rel="noopener"`, avec `aria-label`
  sur les boutons icônes (Facebook, Discord).
- **Images** : `alt` explicites ; SVG décoratifs `aria-hidden="true"`.

## 8. Maintenance courante

- **Changer une couleur** → éditer les variables dans `:root` (unique point
  d'entrée, ex. ligne 60–91).
- **Ajouter une date de soirée** → nouvelle ligne `.date-row` dans la colonne
  correspondante de la section « agenda ».
- **Ajouter un lieu partenaire** → nouvelle `.venue-card` avec image, badge et
  la classe `status-active` / `status-new` / `status-pause`.
- **Ajouter un événement** → nouveau `.tl-item` dans la colonne « Il suffit
  d'être là » ou « Ça se prépare en amont » ; classe `urgent` si priorité.
- **Modifier le logo** → remplacer `images/logo_confrerie_blanc.png` (blanc
  pour fonds sombres) ; penser à fournir l'équivalent noir pour les fonds
  clairs.
- **Polices** → déposer les fichiers dans `fonts/Acumin/` et `fonts/Auster/`
  avec les noms attendus par les `@font-face`.

## 9. Choix d'implémentation

- Site **statique mono-fichier** (une seule page, `index.html`), CSS embarqué,
  aucune dépendance externe (les polices Google ont été retirées au profit des
  fichiers locaux).
- Pas de JavaScript requis pour le rendu ; les SVG sont inline.
- Le déroulement des sections fonctionne via ancres natives (`#soirees`,
  `#agenda`, `#evenements`, `#jdr`, `#rejoindre`) — coffre aussi le menu de la
  nav.