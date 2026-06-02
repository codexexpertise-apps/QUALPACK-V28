# QUALPACK V28 — Architecture propre / Sécurité / Démonstration

QUALPACK est une PWA (Progressive Web App) qualité industrielle destinée principalement aux PME agroalimentaires et semi-industrielles qui doivent réaliser des autocontrôles de poids sur préemballés, sans forcément disposer d’une trieuse pondérale.

L’objectif reste simple : aider le terrain à saisir, tracer, contrôler et exporter les autocontrôles qualité de manière claire, rapide et audit-friendly.

---

## Positionnement produit

QUALPACK est :

- un assistant qualité terrain ;
- un outil d’autocontrôle interne ;
- un outil de traçabilité ;
- une aide à la préparation des audits ;
- une solution simple pour PME agroalimentaires.

QUALPACK n’est pas :

- un ERP ;
- un MES lourd ;
- un logiciel de certification réglementaire ;
- un système de supervision industrielle complexe ;
- un outil qui garantit automatiquement la conformité d’un lot.

La décision finale concernant le lot appartient toujours au service Qualité du site.

---

## Objectif de la V28

La V28 marque le passage d’une version de test terrain stabilisée vers une base plus professionnelle et plus maintenable avant commercialisation.

Objectifs principaux :

- architecture fichiers plus claire ;
- séparation du CSS, du JavaScript, des librairies et des assets ;
- sécurisation progressive de l’accès site ;
- meilleure maintenabilité avant diffusion commerciale ;
- préparation du kit de démonstration ;
- conservation stricte de la simplicité terrain.

La V28 n’ajoute pas de nouvelles fonctionnalités métier lourdes. Elle professionnalise la base existante.

---

## Modes disponibles

### 1. START SOLO

Mode simplifié destiné aux petites structures, aux sites mono-ligne ou aux démonstrations terrain rapides.

Fonctionnalités principales :

- création rapide des produits, lignes et détecteurs ;
- saisie des pesées brutes ;
- calcul automatique du poids net avec tare fixe ;
- aide à l’analyse TU1 / TU2 / TNE ;
- ajout progressif de pesées par pas de +5 sans perte des saisies déjà réalisées ;
- contrôle détecteur de métaux ;
- historique ;
- Dashboard Pro ;
- exports PDF et Excel.

La limitation commerciale peut être pilotée via la donnée :

```text
nb_lignes_autorisees
```

dans la table Supabase `sites`.

---

### 2. SITE

Mode destiné aux PME agroalimentaires multi-lignes.

Fonctionnalités principales :

- plusieurs lignes de production ;
- plusieurs détecteurs ;
- import catalogue via Excel ;
- supervision qualité ;
- Dashboard Pro ;
- historique des contrôles ;
- rapports PDF ;
- exports Excel ;
- synchronisation Supabase.

---

### 3. Mode démonstration terrain

Mode réservé à CODEX EXPERTISE pour les démonstrations client.

Objectifs :

- ouvrir rapidement QUALPACK sur smartphone, tablette ou PC ;
- créer ou utiliser des données de démonstration ;
- réaliser un contrôle de pesées ;
- montrer les résultats, l’historique, le Dashboard Pro et les exports ;
- réinitialiser la démo après présentation.

Le mode démonstration utilise :

```text
site_id = qualpack_demo
mode_demo = true
```

Le bouton :

```text
Réinitialiser la démo
```

est réservé au site de démonstration.

---

## Fonctionnalités principales

### Pesées préemballés

- saisie des poids bruts ;
- déduction de la tare fixe ;
- calcul des poids nets ;
- moyenne nette ;
- seuils TU1 / TU2 ;
- TNE ;
- verdict indicatif d’autocontrôle ;
- ajout de pesées supplémentaires par bouton +5 avant calcul final.

### Sécurité de saisie opérateur

- aide visuelle pendant la saisie ;
- mise en évidence des valeurs inhabituelles ;
- conservation des pesées déjà saisies lors de l’ajout de lignes supplémentaires.

### Contrôles détecteurs

- contrôle des détecteurs associés ;
- historique des tests ;
- rapports PDF.

### Dashboard Pro

- KPI qualité ;
- conformité ;
- non-conformités ;
- suivi des défauts ;
- historique ;
- cockpit qualité dark industriel.

### Exports

- PDF pesées ;
- PDF détecteurs ;
- export Excel avec mise en couleur des résultats ;
- rapports utilisables pour le suivi qualité et les audits.

---

## Sécurité et accès

L’accès site repose sur une validation Supabase via :

```text
qualpack_validate_site_access()
```

Contrôles utilisés :

- site actif ;
- clé site ;
- date d’expiration ;
- nombre de lignes autorisées ;
- mode démonstration ;
- cloisonnement par `site_id`.

Évolutions V28 déjà intégrées :

- génération d’un `access_token` côté Supabase ;
- validation d’accès renforcée ;
- message clair en cas de clé incorrecte ;
- champ clé avec œil / œil barré ;
- bouton `Quitter QualPack` ;
- expérience utilisateur inchangée pour les sites pilotes.

Objectif sécurité final V28 :

- ne plus exposer les clés d’accès côté frontend ;
- empêcher la lecture directe des clés par le client ;
- conserver une validation simple pour l’utilisateur ;
- préparer une authentification utilisateurs plus avancée plus tard.

---

## Architecture technique V28

- Frontend : GitHub Pages
- Backend : Supabase
- Stockage local : IndexedDB
- Synchronisation : Supabase REST
- PWA compatible smartphone / tablette / PC
- Fonctionnement terrain avec logique locale
- Génération PDF locale
- Import / export Excel

---

## Arborescence V28

```text
/
├─ index.html
├─ app.html
├─ demo.html
├─ manifest.json
├─ manifest-demo.json
├─ sw.js
├─ README.md
│
├─ css/
│  └─ style.css
│
├─ js/
│  ├─ app.js
│  ├─ admin.js
│  ├─ db.js
│  ├─ sync.js
│  ├─ pdf-v2.js
│  └─ lignes.js
│
├─ vendor/
│  ├─ jspdf.umd.min.js
│  └─ xlsx.full.min.js
│
└─ assets/
   ├─ icon-192.png
   ├─ icon-192-maskable.png
   ├─ icon-512.png
   ├─ logo-codex.png
   ├─ picto-codex.jpg
   └─ dashboard-pro.png
```

Principes retenus :

- `app.html` reste le point d’entrée principal de l’application ;
- `css/style.css` contient les styles extraits de l’ancien `app.html` ;
- `js/app.js` contient le JavaScript principal extrait de l’ancien `app.html` ;
- `js/admin.js`, `js/db.js`, `js/sync.js`, `js/pdf-v2.js` et `js/lignes.js` restent séparés ;
- `vendor/` contient les bibliothèques externes ;
- `assets/` contient les images et icônes ;
- `sw.js` reste à la racine pour conserver un périmètre PWA correct.

Les anciens fichiers de travail `_inline_app.js` et `_inline_app2.js` ont été retirés de la version V28.

---

## Kit de démonstration prévu

La V28 est préparée pour recevoir un kit de démonstration composé de :

- captures écran propres ;
- exemple de rapport PDF pesées ;
- exemple de rapport PDF détecteur ;
- exemple de rapport qualité ;
- mini vidéo de démonstration ;
- support visuel commercial simple.

Le mode démonstration terrain reste la vraie démonstration interactive.

Le kit de démonstration viendra en complément pour présenter rapidement QUALPACK à un responsable qualité, un auditeur ou une PME agroalimentaire.

---

## Limites volontaires de la V28

La V28 reste volontairement simple.

Ne sont pas encore inclus :

- paiement en ligne ;
- facturation automatique ;
- workflows SaaS complets ;
- MES complet ;
- ERP ;
- automatisation commerciale complète.

QUALPACK doit rester simple, crédible et utile au terrain.

---

## Roadmap future

Pistes conservées pour plus tard :

- authentification utilisateurs Supabase ;
- rôles opérateur / responsable qualité / administrateur ;
- domaine QUALPACK dédié ;
- packaging commercial START / SITE ;
- vidéo courte de démonstration ;
- stabilisation longue durée ;
- éventuelle extension multi-sites plus avancée.

---

## CODEX EXPERTISE

Développement : CODEX EXPERTISE  
Président : Serge Crocilli

Domaines d’expertise :

- contrôle qualité industriel ;
- pesage ;
- détection ;
- vision industrielle ;
- optimisation de chaînes de production agroalimentaires.

