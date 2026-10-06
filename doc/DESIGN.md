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

L'accueil inclut actuellement un **écran d'ouverture événementiel** dédié à
Halloween 2026 : une affiche plein écran bloque brièvement la page et propose
un accès direct à la page de l'événement (la réservation se fait ensuite sur
HelloAsso) avant de laisser l'utilisateur continuer vers le contenu principal.

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
| `--line-faint`    | `rgba(16,16,19,.08)` | Liserés légers sur cartes et cadres de logos.            |
| `--white-90`      | blanc translucide | Texte quasi principal sur fonds sombres / CTA sociale.       |
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
- Copies d'exploitation : `images/logo_confrerie_blanc.png` pour les fonds
  sombres, `images/logo.png` pour le favicon / les métadonnées sociales.
  `images/logo_confrerie_noir.png` n'est plus référencé (il servait au dé
  animé de l'ancien hero) : conservé comme variante de repli.
- Usages : mark de la nav (30×30), logo statique du hero et icône du site.
  Sur les fonds clairs, seule la variante **noire** du logo doit être
  utilisée ; la variante blanche est réservée aux fonds sombres.
- Le logo des partenaires est fourni en `images/` (logos clampés dans des
  cadres blancs de 96px / 108px).

## 5. Composition

- Conteneur : `--max-w` 1080px, espacement latéral 28px, sections
  `padding: 84px 0`.
- Chaque section commence par un **en-tête structuré** : `section-tag`
  (sur-ligne), `h2` très gras avec une **barre de 3px sous le titre**
  (rouge sur fond clair, orange sur fond sombre), puis un paragraphe
  d'introduction.
- Une **splash screen** peut précéder la navigation principale pour pousser un
  événement prioritaire. Elle reste centrée, bloque le scroll via
  `body.is-splash-locked` et se ferme par bouton, clic sur le fond ou touche
  `Escape`.
- Rythme clair/sombre :
  - **Sombre** (noir) : nav, hero, « Événements », CTA, footer.
  - **Clair** (blanc) : « Soirées », « Agenda », « Jeu de rôle ».
- Les tags et accents passent au **orange** sur fond sombre, au
  **rouge accessible** sur fond clair.
- Points de rupture : **860px** (colonnes empilées en 1 colonne, nav en
  colonne) puis **560px** (liens de nav sur 2 colonnes, padding réduit). La nav
  de l'accueil n'a pas de menu burger ; celui-ci est propre à la page
  Halloween, qui masque aussi son visuel d'affiche sous 860px.

## 6. Composants clés

| Composant        | Description                                                               |
| ---------------- | ------------------------------------------------------------------------- |
| Splash Halloween | Affiche plein écran sur fond noir, CTA vers la page de l'événement +     |
|                  | bouton de fermeture.                                                      |
| Nav              | Sticky, fond noir, mark logo + mot, liens en pilules bordées.             |
| Hero             | Fond noir sans cadrillage de points, halos orange/violet, titre 900,     |
|                  | logo **statique** en aside + liste de temps forts.                       |
| Boutons          | Primaire rouge `--red-mid` (hover `--red-deep`), ghost sur fond sombre,  |
|                  | outline violet sur fond clair, bouton Discord dédié, hover avec élévation.|
| Cartes de salles | Tuile blanche, bord supérieur 4px coloré par **statut** :                |
|                  | actif = `--red-mid`, nouveau = `--purple`, pause = `--ink-third`.        |
| Agenda           | Deux colonnes (Dream Team / Bowling) avec points `--red-mid` / `--purple`,|
|                  | horaires en `tabular-nums`, dates passées barrées automatiquement.        |
| Timeline         | Fil noir sur fond sombre, pastilles orange. La variante **d'urgence**     |
|                  | (puce, date et lieu en rouge + badge `.tl-flag`) existe en CSS mais n'est |
|                  | plus appliquée : à réactiver avec `urgent` + `.tl-flag` si un événement  |
|                  | doit ressortir.                                                           |
| Spotlight        | Carte événement photo/copy ; variante `is-archive` pour les événements   |
|                  | passés ( fond translucide, kicker en blanc 70%).                          |
| JDR              | Colonne texte + carte violette en pointillés (`border dashed`).          |
| CTA / footer     | Fonds noirs, chips en pilules, intro sociale animée, icônes WhatsApp /     |
|                  | Facebook / Discord.                                                       |

## 7. Accessibilité

- **Contrastes** : toutes les associations texte/couleur visent le niveau AA.
  Rouge charte réservé au décor (voir § 2). Noir `#000`/blanc `#FFF` pour le
  principal, `#101013` sur blanc pour le corps.
- **Focus visible** : `:focus-visible` = contour orange 2px + offset 2px,
  appliqué globalement.
- **Mouvement** : `prefers-reduced-motion: reduce` coupe le défilement fluide
  et toutes les animations/transitions, y compris la pastille pulsée de
  l'intro sociale, le zoom des visuels et les transitions de boutons.
- **Liens externes** : `target="_blank" rel="noopener"`, avec `aria-label`
  sur les boutons icônes (WhatsApp, Facebook, Discord).
- **Images** : `alt` explicites ; SVG décoratifs `aria-hidden="true"`.
- **Overlay événementiel** : fermeture possible au clavier (`Escape`) et focus
  initial dirigé vers l'action principale.

## 8. Maintenance courante

- **Changer une couleur** → éditer les variables dans `:root` (unique point
  d'entrée dans `index.html`).
- **Ajouter une date de soirée** → nouvelle ligne `.date-row` dans la colonne
  correspondante de la section « agenda », avec l'attribut `data-date`
  (`YYYY-MM-DD`) pour que le barré automatique fonctionne.
- **Ajouter un lieu partenaire** → nouvelle `.venue-card` avec image, badge et
  la classe `status-active` / `status-new` / `status-pause`.
- **Ajouter un événement** → nouveau `.tl-item` dans l'une des deux colonnes
  de `.timeline-groups` (classées par ordre chronologique) ; ajouter `urgent`
  et un `<span class="tl-flag">` si l'événement doit être mis en avant.
- **Mettre à jour le splash événementiel** → remplacer `images/halloween-2026.jpg`
  (1058×1486, ~310 Ko : ne pas repasser en PNG, l'affiche est chargée 2× sur
  l'accueil — splash + spotlight — et pèse le LCP) et ajuster les liens
  `#halloween-enter` / la spotlight si la campagne change. À faire après
  l'événement : la splash et la spotlight Halloween sont à retirer ou repasser
  en `is-archive`.
- **Modifier le logo** → remplacer `images/logo_confrerie_blanc.png` (nav et
  hero) et `images/logo.png` (favicon / OG) selon le besoin.
- **Polices** → déposer les fichiers dans `fonts/Acumin/` et `fonts/Auster/`
  avec les noms attendus par les `@font-face`.

## 9. Choix d'implémentation

- Site **statique**, deux pages en prod (`index.html` + `halloween-2026/index.html`),
  CSS embarqué dans chaque fichier, aucune dépendance externe (les polices Google
  ont été retirées au profit des fichiers locaux). Les fichiers `index-old*.html`
  sont un historique de travail, à ne pas modifier.
- Un **JavaScript minimal inline** pilote deux comportements : fermeture de la
  splash screen Halloween et barré automatique des dates passées via
  `data-date` / `data-end-date`.
- Les SVG sont inline.
- Le déroulement des sections fonctionne via ancres natives (`#soirees`,
  `#agenda`, `#evenements`, `#jdr`, `#rejoindre`), comme le menu de la nav.