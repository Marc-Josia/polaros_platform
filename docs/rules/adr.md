## Décisions d'architecture (ADR)

Toute décision structurante (schéma, RLS, extension Postgres, conventions de migration, outillage) est consignée dans un ADR sous `docs/devlog/ADR/`.

- **Fichier** : `NNN-slug-kebab-case.md` (numéro 3 chiffres, séquentiel, jamais réutilisé).
- **Contenu** : en-tête `Statut` + `Date`, puis `## Contexte`, `## Décision`, `## Conséquences`.
- **Statuts** : `Proposé`, `Accepté`, `Remplacé par [ADR-XXX]`, `Déprécié`.
- **Immuable** : jamais supprimé ni réécrit sur le fond. S'il devient obsolète, changer le statut et créer un nouvel ADR.
- **Quand** : dès qu'une décision a une justification non triviale et un coût de revirement. En cas de doute, en créer un.

---

## Exemple

Le fichier ci-dessous (`docs/devlog/ADR/001-rls-par-defaut-sur-toutes-les-tables.md`) montre la structure attendue. À reproduire tel quel pour chaque nouvel ADR.

```markdown
# ADR-001 — RLS activé par défaut sur toutes les tables

- **Statut** : Accepté
- **Date** : 2026-06-16

## Contexte

L'application expose la base Postgres directement via l'API Supabase (PostgREST).
Sans Row Level Security (RLS), toute table accessible par la clé `anon` est
lisible et modifiable par n'importe quel client. Plusieurs tables contiennent
des données personnelles soumises au RGPD. Le coût d'un oubli d'activation de
RLS sur une seule table est une fuite de données potentielle.

## Décision

Activer `ENABLE ROW LEVEL SECURITY` sur **toutes** les tables du schéma `public`
dès leur création, dans la même migration. Aucune table n'est exposée sans au
moins une policy explicite. Une table sans policy reste donc inaccessible par
défaut (deny-all), ce qui est le comportement recherché.

## Conséquences

- Toute nouvelle table doit inclure son activation RLS et ses policies dans la
  migration de création — sinon elle est inaccessible côté client.
- Les accès de service (tâches d'administration) passent par la clé
  `service_role`, qui contourne RLS : à réserver au back-end.
- Un test de migration vérifie qu'aucune table de `public` n'a RLS désactivé.
```
```