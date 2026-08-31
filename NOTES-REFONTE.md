# Notes de reprise — refonte bilingue

Ce dossier remplace votre dépôt existant. Le site compile sans erreur ni conflit
(testé avec Jekyll 4.3.2).

## Comment le site est organisé maintenant

Chaque page vit en deux exemplaires, dans `_pages/en/` et `_pages/fr/`, et porte
deux champs dans son front matter :

- `lang` : la langue de la page (`en` ou `fr`) ;
- `ref` : un identifiant **partagé** entre les deux versions d'une même page.

Le sélecteur de langue cherche simplement la page qui a le même `ref` dans
l'autre langue. Si vous ajoutez une page, donnez-lui un `ref` identique dans les
deux langues, sinon le sélecteur renverra vers l'accueil.

`lang` est attribué automatiquement par `_config.yml` (`en` par défaut, `fr` pour
tout ce qui est sous `_pages/fr`), donc vous pouvez l'omettre — mais le laisser
explicitement ne coûte rien et rend les fichiers plus lisibles.

| ref | anglais | français |
|---|---|---|
| home | `/en/` | `/fr/` |
| research | `/en/research/` | `/fr/recherche/` |
| publications | `/en/publications/` | `/fr/publications/` |
| teaching | `/en/teaching/` | `/fr/enseignement/` |
| data | `/en/data/` | `/fr/donnees/` |
| cv | `/en/cv/` | `/fr/cv/` |

La racine `/` redirige vers `/en/`. Vos anciennes URLs (`/research/`,
`/publications/`, `/teaching/`, `/data/`, `/cv/`, `/about/`) redirigent vers leur
équivalent anglais grâce à `jekyll-redirect-from`, déjà présent dans vos plugins.

## Fichiers modifiés

- `_config.yml` — description du site, `og_image`, emplacements pour les profils
  académiques (en commentaire), valeurs `lang` par défaut, collections
  désactivées.
- `_data/navigation.yml` — deux menus, `main_en` et `main_fr`.
- `_data/authors.yml` — profil `alex_fr` pour la barre latérale en français.
  **Pensez à tenir les deux profils à jour** : l'anglais est dans `_config.yml`
  sous `author:`, le français ici.
- `_data/ui-text.yml` — « Suivez moi » remplacé par « Suivre ».
- `_includes/lang-switcher.html` — **nouveau**, le sélecteur.
- `_includes/masthead.html` — menu par langue + insertion du sélecteur.
- `_includes/head.html` — balises `hreflang` et `x-default`.
- `_includes/author-profile.html` — liens HAL et SSRN ajoutés, bouton « Suivre »
  traduit, texte alternatif de la photo corrigé.
- `_layouts/default.html` — attribut `lang` du document.
- `_sass/layout/_custom.scss` — **nouveau**, styles du sélecteur et des résumés
  repliables.
- `assets/css/main.scss` — importe le fichier ci-dessus.
- `index.html` — **nouveau**, redirection de la racine.

Dans tous les includes et layouts, `site.data.ui-text[site.locale]` est devenu
`site.data.ui-text[page.lang]` (65 occurrences), pour que les libellés
d'interface suivent la langue de la page.

## Ce qui a été supprimé

Le contenu de démonstration d'AcademicPages : `_posts`, `_drafts`, `_portfolio`,
`_publications`, `_talks`, `_teaching`, les pages `markdown.md`, `terms.md`,
`portfolio.html`, `talks.html`, les archives par tag et catégorie, le talkmap et
les fichiers `paper1.pdf`, `slides1.pdf`, `bio-photo.jpg`, etc. Tout cela
apparaissait publiquement sur votre `/sitemap/`.

Au passage, `_pages/publications.html` et `_pages/publications.md` écrivaient
tous les deux vers `/publications/index.html` — Jekyll signalait le conflit à
chaque build. Idem pour `teaching`. C'est réglé.

## À faire de votre côté

1. **Déposer `files/cv-alex-amiotte-suchet.pdf`.** Quatre boutons pointent vers
   ce fichier, qui n'existe pas encore. Sinon, retirez les boutons.
2. **Compléter les pages CV** (`_pages/en/cv.md`, `_pages/fr/cv.md`). Toutes les
   sections vides sont marquées `TODO` / `À COMPLÉTER` en commentaire HTML : je
   n'ai rempli que ce qui figurait déjà sur le site, et rien inventé sur votre
   formation, votre thèse, vos communications ou vos financements.
3. **Renseigner le titre et la direction de thèse** sur les pages recherche.
4. **Relire les résumés français des working papers**, que j'ai traduits depuis
   l'anglais (signalés par un commentaire dans `_pages/fr/recherche.md`).
5. **Activer les profils académiques** : décommentez les lignes voulues dans
   `_config.yml` (`author:`) *et* dans `_data/authors.yml` (`alex_fr`).
   ORCID, HAL, SSRN, Google Scholar, LinkedIn et Bluesky sont prêts.
6. **Mettre à jour le fil « Actualités »** sur les deux pages d'accueil.

Suggestions laissées en commentaire dans les fichiers concernés : descriptions
de cours et syllabus sur les pages enseignement, dépôt Zenodo/Nakala avec DOI et
licence sur les pages données.

## Tester en local

```
bundle install
bundle exec jekyll serve
```

Puis ouvrez http://localhost:4000. Si `jemoji` pose problème à l'installation,
vous pouvez le retirer du `Gemfile` : il n'est utilisé nulle part sur le site.
