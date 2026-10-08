# Site Emile Garçon — (Re)Born : consignes pour Claude

Ce dépôt est la **source de référence** du site. Toute modification se fait ici, puis est poussée sur `main` :
GitHub Pages republie le site automatiquement (environ 1 minute). Domaine prévu : emilegarcon.com.

## Règles de travail
- Répondre en français, directement, sans flatterie. Indiquer le niveau de confiance quand c'est incertain.
- Ne jamais utiliser la tournure « ce n'est pas X, c'est Y ».
- Après chaque modification : tester (Playwright, Chromium dans `/opt/pw-browsers/chromium`), commit, `git push origin main`.
- Le site est bilingue FR/EN : tout texte visible passe par `x("français","english")`.
- **Ne jamais placer deux poses identiques côte à côte** dans une grille (voir `MODEL`, `POSE`, `spread()`).
- Dans les vignettes, la description ne dépasse pas 3 lignes.
- Ne pas écrire de mentions juridiques inventées : une info manquante s'écrit `T("…")` ou `tbd("fr","en")`.

## Structure (tout est dans `index.html`, une seule page)
- Routeur par hash : `render()` appelle `V.home`, `V.pieces`, `V.piece`, `V.couture`, `V.creation`, `V.press`,
  `V.legal`, `V.cgv`, `V.ship`, `V.reborn`, `V.maison`, `V.account`… Le paramètre `?s=<id>` fait défiler jusqu'à une section.
- `P` : pièces (Re)Born. Champs : `h` (identifiant = handle Shopify), `t`, `type`, `cat`, `taille`, `sku`, `prix`,
  `etat` (`dispo` / `showroom` / `vendue` / `bonmarche`), `d`, `mat`, `tech`, `motif`, `ext:[{k,alt}]`, `int`.
- `C` : collection Couture (même logique, champ `status`).
- `IMG` : clé → chemin `img/*.webp`. Images de presse dans `img/presse/`.
- `PRESS` : articles de presse. `STATES`, `buyable()` : états de vente.
- Panier : `bag` (localStorage), `drawCart()`, `goCheckout()`.

## Shopify (boutique iwkwfi-et.myshopify.com, nom « Emile Garçon »)
- Le bouton « Passer commande » ouvre le paiement Shopify via un lien de panier :
  `https://iwkwfi-et.myshopify.com/cart/<variantId>:1,…`
- `SHOP.vid` relie chaque handle du site à l'identifiant numérique de sa variante Shopify.
  **Toute nouvelle pièce doit y être ajoutée**, sinon elle ne peut pas être payée.
- `SHOP.token` : jeton public Storefront API (canal Headless), vide pour l'instant. S'il est renseigné, `syncShop()`
  met à jour prix et disponibilité au chargement.
- Créer une pièce : `productCreate` → prix/SKU (`productVariantsBulkUpdate`, politique de stock DENY) →
  stock 1 sur l'emplacement `gid://shopify/Location/121753895254` (Showroom) → photos et textes alternatifs →
  publication sur les canaux 356149526870 (Boutique en ligne), 356149559638 (Shop), 356149592406 (Point de vente).
- Pièce vendue : `etat:"vendue"` sur le site (ou retrait), produit **archivé** dans Shopify avec un stock à 0.
- Ce qui n'est pas sur le site ne doit plus être disponible sur Shopify.

## Ajouter une pièce (procédure habituelle)
1. Convertir les photos en `.webp` dans `img/`, puis les ajouter à `IMG`.
2. Ajouter l'entrée dans `P` (prénom masculin d'époque comme nom, taille, prix, matières, description FR/EN).
3. Créer le produit Shopify, puis reporter l'identifiant de variante dans `SHOP.vid`.
4. Vérifier qu'aucune pose identique n'est voisine. Tester, commit, push.

## Reste à compléter avant lancement
- CGV et page Livraison : transporteur et délai d'envoi.
- Médiateur de la consommation (obligatoire).
- Hébergeur à confirmer (GitHub Pages pour le site, Shopify pour le paiement), cookies, dates des pages légales.
- Deux récits « Récit à écrire », page Couture (origine, point de départ, cadre photo vide).
- Presse : liens et certaines dates.
- Faute sur l'étiquette d'Honoré (« Parc qu'il y a »).
- Étole Velours & Soie : masquée, brouillon dans Shopify.
- Page Compte : simulée (à relier aux comptes clients Shopify).
