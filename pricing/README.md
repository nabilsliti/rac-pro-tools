# pricing.json — config tarifaire publique `rac-pro`

Ce fichier est la **source de vérité publique** des tarifs des modules du
plugin WordPress [`rac-pro`](https://github.com/). Le plugin fetch ce JSON
au runtime pour afficher les prix dans son écran d'admin et tombe en
fallback sur un bundle local en cas d'erreur réseau.

## URL publique

Après activation de GitHub Pages sur ce repo :

```
https://<owner>.github.io/rac-pro-tools/pricing/pricing.json
```

Remplacer `<owner>` par le propriétaire GitHub du repo.

## Modifier un prix

1. Éditer `pricing/pricing.json`
2. Mettre à jour `updated_at` (ISO 8601 UTC, ex: `2026-06-15T14:30:00Z`)
3. Ajouter une ligne datée dans `pricing/CHANGELOG.md`
4. Commit + push sur `main`
5. Live sous ~30s (le temps que GitHub Pages redéploie)

Valider le JSON avant de pusher :

```bash
jq . pricing/pricing.json
```

## Schéma

| Champ | Type | Description |
|---|---|---|
| `version` | entier | Version du **schéma**. À incrémenter UNIQUEMENT en cas de breaking change de structure (le plugin peut s'en servir pour ignorer un format inconnu). |
| `updated_at` | string ISO 8601 UTC | Horodatage de la dernière édition. À mettre à jour à chaque commit. |
| `currency` | string ISO 4217 | Code devise (`EUR`, `TND`, `USD`…). |
| `modules.<slug>.annual` | number | Prix annuel public hors promo, dans la devise `currency`. |
| `modules.<slug>.monthly_eq` | number | Équivalent mensuel affiché en sous-texte (`annual / 12`, arrondi éditorialement). Purement cosmétique. |
| `modules.<slug>.save` | number | Économie éditoriale affichée par rapport à un prix mensuel de référence. Calculée en dehors, simple chiffre marketing. |
| `promo.active` | bool | Si `true` ET `ends_at` non dépassé, le plugin applique `discount_pct` au prix `annual` à l'affichage. |
| `promo.label` | string | Texte court affiché en badge (ex: `"Black Friday -30%"`). |
| `promo.discount_pct` | entier 0-100 | Pourcentage de remise appliqué sur `annual`. |
| `promo.ends_at` | string ISO 8601 UTC ou `null` | Date de fin de promo. `null` = pas de date limite (mais nécessite quand même `active: true`). |

### Slugs de modules

`pricing`, `deposit`, `skin`, `invoice`, `notifications`.

Ajouter un nouveau slug ne casse rien côté plugin (il ignore les slugs
qu'il ne connaît pas). Retirer un slug encore référencé côté plugin le
fera tomber en fallback local pour ce module.

## Prudence

Ce fichier est **servi public sans signature**. Conséquences :

- Ne JAMAIS y mettre de données sensibles (clés API, secrets, prix
  internes négociés…).
- Considérer son contenu comme purement éditorial et public.
- Tout client peut lire et cacher ce fichier — un changement de prix
  n'est donc pas instantané pour les utilisateurs ayant déjà mis en
  cache la réponse.
