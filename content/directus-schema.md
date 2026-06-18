# Schéma Directus prévu (préparation, non connecté)

Ce document décrit les futures collections Directus + PostgreSQL qui remplaceront
`content/site-content.json`. Le site reste statique tant qu'aucune de ces collections
n'est réellement créée/connectée — ce fichier sert uniquement de référence de design.

## Collections prévues

### `pages`
- `slug` (string, unique) — ex. `index`, `about`, `marques`
- `title` (string)
- `path` (string) — fichier html correspondant
- `seo_title`, `seo_description`, `canonical_url`
- `sections` (relation M2M vers `sections`)

### `sections`
- `key` (string, unique) — ex. `home_hero`, `about_timeline`
- `page` (relation M2O vers `pages`)
- `type` (string) — `hero`, `cards`, `timeline`, `cta`, `list`
- `heading`, `body` (text)
- `items` (json ou relation O2M selon le type)

### `produits`
- `slug`, `name`, `description`
- `category` (relation M2O vers `categories`)
- `marque` (relation M2O vers `marques`)
- `images` (relation O2M vers fichiers Directus)
- `fiche_technique` (relation vers `documents`)
- `disponibilite` (string/enum)

### `categories`
- `slug`, `name`, `description`, `icon`

### `marques`
- `slug`, `name`, `url`, `logo`, `categories` (M2M)
- `description`

### `faq`
- `question`, `reponse`, `categorie`, `ordre`

### `blog`
- `slug`, `title`, `excerpt`, `body`, `cover_image`, `published_at`, `author`

### `documents`
- `title`, `file`, `category` (notice, fiche technique, certificat...)
- `produit` (relation M2O optionnelle)

### `codes_erreurs`
- `marque` (relation M2O vers `marques`)
- `code`, `description`, `solution`

### `demandes_contact`
- `nom`, `email`, `telephone`, `message`, `page_origine`, `created_at`, `statut`

## Migration future

1. Créer les collections ci-dessus dans Directus (PostgreSQL en backend).
2. Importer le contenu de `content/site-content.json` comme données initiales.
3. Remplacer la lecture statique du JSON par des appels à l'API Directus
   (REST ou GraphQL) dans les pages concernées.
4. Conserver les mêmes clés (`slug`, `key`) pour ne pas casser les ancres déjà
   utilisées dans le HTML (ex. `marques.html#hirsch`).

Aucune connexion Directus/PostgreSQL n'est active à ce stade — uniquement de la préparation.
