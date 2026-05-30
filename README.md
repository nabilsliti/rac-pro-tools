# rac-pro-tools

Repo outils & configs publiques pour le plugin WordPress
[`rac-pro`](https://github.com/).

## Contenu

- [`pricing/pricing.json`](pricing/pricing.json) — config tarifaire
  publique consommée par le plugin. Voir [`pricing/README.md`](pricing/README.md).

## Activation GitHub Pages

Ce repo publie son contenu via GitHub Pages. Deux options possibles :

### Option A — GitHub Actions (recommandée, déjà configurée)

Le workflow [`.github/workflows/pages.yml`](.github/workflows/pages.yml)
upload tout le repo comme artifact Pages et le déploie à chaque push sur
`main` modifiant `pricing/`.

1. **Settings → Pages**
2. **Source** : `GitHub Actions`
3. Push sur `main` → le workflow tourne → Pages live sous ~1-2 min.

### Option B — Deploy from a branch

Si tu préfères ne pas utiliser de workflow :

1. **Settings → Pages**
2. **Source** : `Deploy from a branch`
3. **Branch** : `main`, **Folder** : `/ (root)`
4. **Save** → attendre 1-2 minutes.

Tu peux alors supprimer `.github/workflows/pages.yml`.

## Vérification

Une fois Pages activé, l'URL publique :

```
https://<owner>.github.io/rac-pro-tools/pricing/pricing.json
```

doit répondre en `200` avec le JSON brut. GitHub Pages sert
automatiquement le `Content-Type: application/json` pour les fichiers
`.json`.

```bash
curl -i https://<owner>.github.io/rac-pro-tools/pricing/pricing.json
```

## Contraintes

- Pas de build step — JSON statique servi tel quel.
- Repo **public** obligatoire (GitHub Pages gratuit ne sert pas les
  repos privés).
- Pas de framework, pas de dépendances NPM.
- Valider `pricing.json` avant chaque commit : `jq . pricing/pricing.json`.
