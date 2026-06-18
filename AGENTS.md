# Instructions pour les agents IA

Polaros est une AI Native Service Company qui propose un back-office commercial externalisé pour les PME de terrain : une équipe hybride (agents IA vocaux + commerciaux humains) qui prend en charge les appels entrants que les vendeurs du client ne peuvent pas décrocher, qualifie les leads selon ses critères, cale des rendez-vous dans son agenda et alimente son CRM — le tout vendu en abonnement récurrent.

## Structure du projet

- NE JAMAIS enregistrer à la racine — utiliser les répertoires ci-dessous
- Utiliser `/src` pour les fichiers de code source
- Utiliser `/tests` pour les fichiers de test
- Utiliser `/docs` pour la documentation et les fichiers markdown
- Utiliser `/config` pour les fichiers de configuration
- Utiliser `/scripts` pour les scripts utilitaires
- Utiliser `/examples` pour le code d'exemple
- Utiliser `/temp` pour les fichiers temporaires

## Conventions

- Commence toujours tes message par mon nom "Marc"
- Toujours répondre à l'utilisateur, rédiger les commit et PR en français
- Toujours rédiger le code, les url, les routes, nom de fichier en anglais
- Réponds le plus court possible : aucune phrase de remplissage, va droit au but
- Une seule question à la fois, jamais plusieurs dans le même message
- Toute décision structurante (schéma, RLS, extension Postgres, conventions de migration, outillage) est consignée dans un ADR sous `docs/devlog/ADR/`. Suivre @docs/rules/adr.md


## Limites

- Toujours : respecter les règles de @docs/rules/security.md
