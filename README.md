**[English](#counter-tray-generator) · [Français](#générateur-de-plateau-à-jetons)**

---

# Counter Tray Generator

## Purpose

This web page is a **parametric generator for a counter/token storage tray** (useful for board games, wargames, etc.), shipped as a **single self-contained HTML file** — no install, no server: everything runs in the browser.

The tool lets you:

- adjust about thirty parameters (dimensions, wall thicknesses, roundings, notches…) through sliders and numeric fields;
- preview the result in **real-time 3D** (orbit view: iso / top / front);
- see a matching **2D slot plan** side by side;
- generate either the **tray** or its **lid**;
- **export the model as STL** (binary), ready for 3D printing;
- share a configuration via a **link that encodes the parameters** (in the URL hash);
- work in **French or English** (instant interface switch).

The 3D geometry is computed with the **[JSCAD](https://github.com/jscad/OpenJSCAD.org)** library, entirely in client-side JavaScript.

The generated tray is a grid of slots (one slot = one storage spot for a stack of tokens); each slot can have a window in its base (to push tokens up from below) and a side opening toward the neighbouring slot (to grab the stack more easily). The lid caps the tray, with an adjustable fit clearance and a notch for easy removal.

## Getting started with the interface

| Element | Role |
|---|---|
| **Tray / Lid** | Chooses which part to generate and export. |
| **Language (EN/FR)** | Switches the whole interface's language. |
| **Copy link** | Copies the current URL, which encodes all current settings (handy for sharing or coming back to it later). |
| **Reset** | Restores the default values. |
| **Iso / Top / Front views** | Reframes the 3D preview camera. |
| **Slot plan** | 2D preview of the slot grid. |
| **Download STL** | Exports the currently displayed part as a binary STL file. |

A warning banner may appear if the current settings produce fragile or overly heavy-to-compute geometry (see the *Warnings* section below).

## Parameters

The parameters are grouped into four sections in the left-hand panel.

### 1. Overall size (`size`)

Defines the number of slots, either directly or by deriving it from a maximum overall dimension.

| Parameter | Unit / range | Role |
|---|---|---|
| **Maximum length with lid** (`maxLength`) | mm, 20–400 | Maximum outer length allowed for the assembled tray + lid. Used to automatically work out the number of slots along the length when that count is 0. |
| **Maximum width with lid** (`maxWidth`) | mm, 20–400 | Same, for the maximum outer width. |
| **Slots along length** (`cols`) | 0–30 (0 = auto) | Number of slots along the tray's length. At 0, this is derived from the maximum length; entering a value manually takes precedence instead, and the final length of the part follows it. |
| **Slots along width** (`rows`) | 0–30 (0 = auto) | Same logic as above, for the width. |

> Tip: leave `cols`/`rows` at 0 to let the tool fill the target envelope as well as possible; set them yourself to lock in an exact slot count, even if that means exceeding the target envelope.

### 2. Counters (`counters`)

Describes the stack of tokens each slot needs to hold.

| Parameter | Unit / range | Role |
|---|---|---|
| **Counter length** (`counterLength`) | mm, 5–100 | Size of one slot along the tray's length direction. |
| **Counter width** (`counterWidth`) | mm, 5–100 | Size of one slot along the tray's width direction. Equal values give square counters. |
| **Stack height** (`counterStack`) | mm, 2–60 | Usable height of each slot, i.e. the height of the token stack it can hold; drives the height of the tray walls. |

> Tip: Add 1mm to each parameter to allow a little space between the pieces and the board, making them easier to handle.

### 3. Structure

Wall thicknesses, roundings, and openings that define the structure of the part.

| Parameter | Unit / range | Role |
|---|---|---|
| **Base thickness** (`thickness`) | mm, 0.4–5 | Thickness of the tray's (or lid's) base plate. |
| **Outer wall** (`outerWall`) | mm, 0.4–5 | Thickness of the tray's outer frame. |
| **Dividers** (`innerWall`) | mm, 0.4–5 | Thickness of the internal partitions separating the slots. |
| **Rounding radius** (`fillet`) | mm, 0–50 | Fillet radius applied to edges and corners (windows, notches…), for a nicer look and a more pleasant feel. |
| **Base window** (`window`) | × counter, 0–0.8 | Size of the opening cut into the base of each slot, expressed as a proportion of the counter's size. At 0 the base is solid; the higher the value, the easier it is to push tokens out from below. |
| **Opening between slots** (`opening`) | × counter, 0–0.7 | Width of the notch cut into the dividers between two neighbouring slots, as a proportion of the counter's size. Lets you slide a finger over the divider to grab the stack. At 0 the dividers are solid. |

### 4. Lid (`lid`)

Settings specific to the lid; they also feed into the overall envelope used by the automatic slot-count calculation.

| Parameter | Unit / range | Role |
|---|---|---|
| **Lid wall** (`lidWall`) | mm, 0.4–5 | Thickness of the lid's side walls. |
| **Fit clearance** (`lidClearance`) | mm, -0.3–0.6 | Extra gap added between the tray and the lid. A negative value tightens the fit (suited to well-calibrated printers only); a positive value loosens it. |
| **Grip notch** (`lidNotch`) | × length, 0–0.9 | Proportion of the lid's long sides left low (notched) instead of full height, so the tray can be gripped from the sides to lift the lid off easily. |

## Automatic warnings

The tool watches for certain combinations of settings and shows a message when:

- the total slot count exceeds **700** (both the calculation and the print will take a long time);
- the remaining lip around a base window drops below **1.2 mm** (tokens will sit poorly);
- the lid's **fit clearance** is negative (a tight fit, suited to well-calibrated printers only).

If a combination of values yields no valid geometry at all, a message invites you to step back or reset the settings.

## Technical notes

- All 3D generation (JSCAD) and STL export happen **entirely client-side**, with no data sent to any server.
- If the JSCAD library cannot be loaded (no internet connection), the tool shows instructions for using it locally instead.
- If the browser does not expose WebGL, the 3D preview stays empty, but the slot plan and STL export continue to work normally.
- License: MIT (see the header of the `index.html` file).

---

# Générateur de plateau à jetons

## Objectif

Cette page web est un **générateur paramétrique de plateau de rangement pour jetons/pions** (utile pour les jeux de société, wargames, etc.), livré sous forme d'un **fichier HTML autonome** — aucune installation, aucun serveur : tout s'exécute dans le navigateur.

L'outil permet de :

- régler une trentaine de paramètres (dimensions, épaisseurs, arrondis, encoches…) via des curseurs et des champs numériques ;
- visualiser le résultat en **3D en temps réel** (vue orbitale iso / dessus / face) ;
- visualiser en parallèle un **plan 2D des cases** ;
- générer soit le **plateau** (tray), soit son **couvercle** (lid) ;
- **exporter le modèle en STL** (binaire), prêt pour l'impression 3D ;
- partager une configuration via un **lien contenant les paramètres** (encodés dans le hash de l'URL) ;
- travailler en **français ou en anglais** (bascule instantanée de l'interface).

La géométrie 3D est calculée avec la bibliothèque **[JSCAD](https://github.com/jscad/OpenJSCAD.org)**, entièrement en JavaScript côté client.

Le plateau généré est une grille de cases (une case = un emplacement pour une pile de jetons), chaque case pouvant avoir une fenêtre dans le fond (pour pousser les jetons vers le haut) et une ouverture latérale vers la case voisine (pour saisir la pile plus facilement). Le couvercle vient coiffer le plateau, avec un jeu de montage réglable et une encoche pour le retirer facilement.

## Prise en main de l'interface

| Élément | Rôle |
|---|---|
| **Plateau / Couvercle** | Choisit la pièce à générer et à exporter. |
| **Langue (EN/FR)** | Change la langue de toute l'interface. |
| **Copier le lien** | Copie l'URL courante, qui encode tous les réglages en cours (permet de la partager ou de la retrouver plus tard). |
| **Réinitialiser** | Revient aux valeurs par défaut. |
| **Vues Iso / Dessus / Face** | Recadre la caméra de l'aperçu 3D. |
| **Plan des cases** | Aperçu 2D de la grille de cases, avec leurs fenêtres. |
| **Télécharger le STL** | Exporte la pièce actuellement affichée au format STL binaire. |

Un bandeau d'avertissement peut apparaître si les réglages produisent une géométrie fragile ou trop longue à calculer (voir la section *Avertissements* plus bas).

## Paramètres

Les paramètres sont regroupés en quatre sections dans le panneau de gauche.

### 1. Encombrement (`size`)

Définit le nombre de cases, soit directement, soit par déduction à partir d'une taille maximale.

| Paramètre | Unité / plage | Rôle |
|---|---|---|
| **Longueur maximale avec couvercle** (`maxLength`) | mm, 20–400 | Longueur externe maximale autorisée pour l'ensemble plateau + couvercle. Sert à calculer automatiquement le nombre de cases en longueur quand celui-ci est à 0. |
| **Largeur maximale avec couvercle** (`maxWidth`) | mm, 20–400 | Idem pour la largeur externe maximale. |
| **Cases en longueur** (`cols`) | 0–30 (0 = auto) | Nombre de cases le long de la longueur du plateau. À 0, ce nombre est déduit de la longueur maximale ; une valeur saisie manuellement prend le pas, et c'est alors la longueur finale de la pièce qui s'ajuste. |
| **Cases en largeur** (`rows`) | 0–30 (0 = auto) | Même logique que ci-dessus, pour la largeur. |

> Astuce : laissez `cols`/`rows` à 0 pour que l'outil remplisse au mieux le gabarit maximal ; les renseigner soi-même pour figer un nombre de cases précis, quitte à dépasser le gabarit visé.

### 2. Jetons (`counters`)

Décrit la pile de jetons que chaque case doit accueillir.

| Paramètre | Unité / plage | Rôle |
|---|---|---|
| **Longueur du jeton** (`counterLength`) | mm, 5–100 | Dimension d'une case dans le sens de la longueur du plateau. |
| **Largeur du jeton** (`counterWidth`) | mm, 5–100 | Dimension d'une case dans le sens de la largeur du plateau. Valeurs égales = cases carrées. |
| **Hauteur de la pile** (`counterStack`) | mm, 2–60 | Hauteur utile de chaque case, c'est-à-dire la hauteur de la pile de jetons qu'elle peut contenir ; détermine la hauteur des parois du plateau. |

> Astuce : Ajoutez 1mm à chaque paramètre pour laisser un peu de jeu entre les pions et le plateau, afin de faciliter leur manipulation.

### 3. Structure

Épaisseurs, arrondis et ouvertures qui définissent la structure de la pièce.

| Paramètre | Unité / plage | Rôle |
|---|---|---|
| **Épaisseur du fond** (`thickness`) | mm, 0.4–5 | Épaisseur de la plaque de base du plateau (ou du couvercle). |
| **Paroi extérieure** (`outerWall`) | mm, 0.4–5 | Épaisseur du cadre extérieur du plateau. |
| **Séparateurs** (`innerWall`) | mm, 0.4–5 | Épaisseur des cloisons internes qui séparent les cases entre elles. |
| **Rayon d'arrondi** (`fillet`) | mm, 0–50 | Rayon de congé appliqué aux arêtes et coins (fenêtres, encoches…), pour une apparence et un toucher plus agréables. |
| **Fenêtre du fond** (`window`) | × jeton, 0–0.8 | Taille de l'ouverture pratiquée dans le fond de chaque case, exprimée en proportion de la taille du jeton. À 0, le fond est plein ; plus la valeur augmente, plus il est facile de pousser les jetons par en dessous pour les extraire. |
| **Ouverture entre cases** (`opening`) | × jeton, 0–0.7 | Largeur de l'échancrure ménagée dans les cloisons entre deux cases voisines, en proportion de la taille du jeton. Permet de glisser un doigt par-dessus le séparateur pour attraper la pile. À 0, les cloisons sont pleines. |

### 4. Couvercle (`lid`)

Réglages spécifiques au couvercle ; ils influent aussi sur l'encombrement global pris en compte par le calcul automatique des cases.

| Paramètre | Unité / plage | Rôle |
|---|---|---|
| **Paroi du couvercle** (`lidWall`) | mm, 0.4–5 | Épaisseur des parois latérales du couvercle. |
| **Jeu au montage** (`lidClearance`) | mm, -0.3–0.6 | Jeu ajouté entre le plateau et le couvercle. Une valeur négative resserre le contact entre le plateau et le couvercle (réservé aux imprimantes bien calibrées) ; une valeur positive le desserre. |
| **Encoche de préhension** (`lidNotch`) | × longueur, 0–0.9 | Proportion des grands côtés du couvercle laissée basse (échancrée) plutôt que pleine hauteur, pour pouvoir saisir le plateau par les côtés et retirer le couvercle facilement. |

## Avertissements

L'outil surveille certaines combinaisons de réglages et affiche un message si :

- le nombre total de cases dépasse **700** (le calcul et l'impression seront très longs) ;
- le rebord restant autour d'une fenêtre de fond descend sous **1,2 mm** (les jetons tiendront mal dans leur case) ;
- le **jeu au montage** du couvercle est négatif (ajustement serré, réservé aux imprimantes bien calibrées).

Si une combinaison de valeurs ne produit aucune géométrie valide, un message invite à revenir en arrière ou à réinitialiser les réglages.

## Notes techniques

- Toute la génération 3D (JSCAD) et l'export STL se font **côté client**, sans aucun envoi de données vers un serveur.
- Si la bibliothèque JSCAD ne peut pas être chargée (pas de connexion Internet), l'outil affiche des instructions pour l'utiliser en local.
- Si le navigateur n'expose pas WebGL, l'aperçu 3D reste vide mais le plan des cases et l'export STL continuent de fonctionner normalement.
- Licence : MIT (voir l'en-tête du fichier `index.html`).