# Préparer l’entreprise au développement logiciel assisté par agents

## 1. Résumé exécutif

Le développement informatique entre dans une phase où l’outil principal ne sera probablement plus seulement l’IDE ou le chat IA, mais un **orchestrateur d’agents** capables de travailler en parallèle sur plusieurs branches, dans des environnements cloud isolés, avec accès au code, aux tests, aux logs, aux tickets, à la documentation, au design system et aux outils internes.

Le changement central n’est pas simplement que “l’IA code mieux”. Le changement est que l’unité de travail va évoluer :

- avant : écrire du code ligne par ligne ;
- aujourd’hui : demander de l’aide, générer du code, faire des refactors assistés ;
- demain : assigner des tâches bornées à des agents, comparer plusieurs branches candidates, valider des changements, piloter la qualité.

Dans ce contexte, les entreprises qui auront les meilleurs résultats ne seront pas seulement celles qui auront accès aux meilleurs modèles. Ce seront celles qui auront :

- un codebase bien structuré ;
- des conventions explicites ;
- des tests fiables ;
- des environnements reproductibles ;
- une documentation à jour ;
- des règles automatisées ;
- des tickets bien découpés ;
- des développeurs capables de piloter, contrôler et reviewer le travail des agents.

La qualité du contexte deviendra aussi importante que la qualité du prompt.

---

## 2. Historique rapide de l’évolution du développement assisté

### 2.1 Période “Google + Stack Overflow”

Pendant longtemps, le flux de travail principal était :

```text
Erreur → Google → Stack Overflow / GitHub issue → copier-coller → adaptation → test local
```

Le développeur gardait la main sur tout. Les outils l’aidaient surtout à trouver de l’information.

### 2.2 Période autocomplete intelligent

Avec Copilot et les outils similaires, l’IA est entrée dans l’IDE :

```text
Le développeur commence à écrire → l’outil prédit la suite
```

Cette phase a surtout aidé sur :

- boilerplate ;
- fonctions prévisibles ;
- tests simples ;
- mappings de types ;
- petits refactors locaux.

L’outil restait cependant passif : il ne pilotait pas réellement le projet.

### 2.3 Période ChatGPT comme assistant interactif

Ensuite, les développeurs ont commencé à entrer directement leurs problèmes dans ChatGPT :

```text
Je décris mon problème → l’IA propose une solution → je l’adapte dans mon code
```

L’IA est devenue un interlocuteur pour comprendre, expliquer, déboguer et générer du code. Mais il restait beaucoup de copier-coller entre l’IDE, le navigateur, le terminal et les outils projet.

### 2.4 Période tool calling, agents et MCP

La phase suivante est celle des agents capables de :

- lire des fichiers ;
- modifier du code ;
- appeler des outils ;
- exécuter des commandes ;
- lancer les tests ;
- lire les erreurs ;
- corriger leur propre travail ;
- interagir avec des outils externes via MCP ou équivalent.

C’est le passage de l’assistant textuel à l’agent opérationnel.

---

## 3. État de l’art actuel

Aujourd’hui, les développeurs disposent déjà de plusieurs catégories d’outils IA :

- autocomplete dans l’IDE ;
- chat contextuel sur le code ;
- agents CLI ;
- agents cloud ;
- assistants de PR ;
- assistants de review ;
- outils connectés à GitHub, GitLab, Jira, Slack, Figma, Datadog, etc. ;
- MCP servers pour connecter les agents à des sources de contexte et à des outils.

Le développement assisté ne se limite plus à :

```text
Écris-moi cette fonction
```

Il commence à ressembler à :

```text
Prends cette issue, comprends le repo, fais une branche, modifie le code, lance les tests, explique les risques et propose une PR.
```

Le développeur devient progressivement un **pilote de changements** plutôt qu’un simple auteur de code.

---

## 4. Hypothèse centrale : l’orchestrateur d’agents comme outil principal

L’évolution probable est l’apparition d’un cockpit de développement où plusieurs agents travaillent en parallèle.

### 4.1 Modèle attendu

Un orchestrateur pourrait lancer plusieurs agents dans des environnements isolés :

```text
main
├── agent/bugfix-permissions-table
├── agent/refactor-guarded-form
├── agent/playwright-import-answers
├── agent/perf-mapping-table-details
└── agent/spike-apollo-cache-strategy
```

Chaque agent disposerait de :

- sa branche ;
- son sandbox ;
- ses logs ;
- son terminal ;
- ses commandes lancées ;
- ses tests ;
- ses captures UI ;
- son résumé de PR ;
- ses incertitudes ;
- son historique de décisions.

Le développeur humain comparerait les résultats et déciderait quoi garder.

### 4.2 Branches comme hypothèses

Aujourd’hui, une branche correspond souvent à une feature ou à un développeur.

Demain, une branche pourrait correspondre à une **hypothèse de résolution**.

Exemple :

```text
Objectif : améliorer les performances de MappingTableDetails.

Agent A : virtualisation de table
Agent B : mémoïsation ciblée
Agent C : changement de stratégie Apollo cache
Agent D : profilage uniquement
```

L’orchestrateur comparerait ensuite :

- les gains mesurés ;
- les risques ;
- le nombre de fichiers modifiés ;
- les tests ajoutés ;
- la lisibilité ;
- l’impact produit ;
- la compatibilité avec les conventions existantes.

### 4.3 Nouveau rôle du développeur

Le développeur deviendra davantage :

- pilote ;
- reviewer ;
- architecte ;
- garant de cohérence ;
- rédacteur de contraintes ;
- concepteur de tests ;
- arbitre entre plusieurs solutions.

Il continuera à coder, mais son unité de travail principale deviendra moins “la ligne de code” et plus “le changement validé”.

---

## 5. Les grands changements à anticiper

## 5.1 Découper les issues deviendra critique

Plus les agents seront puissants, plus il faudra leur donner des tâches petites, bornées et vérifiables.

Une grosse issue comme :

```text
Refondre la gestion des permissions dans Data Collection
```

est dangereuse pour un agent, car elle lui laisse trop de liberté.

Elle doit être transformée en sous-issues contrôlables :

```text
1. Ajouter un test de non-régression sur l’affichage des boutons selon permission X.
2. Extraire la logique de visibilité dans une fonction pure.
3. Remplacer l’usage inline dans le composant A uniquement.
4. Étendre au composant B.
5. Ajouter une documentation courte du pattern.
```

Le découpage devient une activité de qualité.

### Format recommandé pour une issue agent-compatible

```text
Objectif
- Ce que l’agent doit produire.

Contexte
- Pourquoi on fait ce changement.
- Où se trouve le comportement actuel.

Scope autorisé
- Fichiers ou dossiers modifiables.

Scope interdit
- Ce que l’agent ne doit pas toucher.

Contraintes techniques
- Patterns à suivre.
- APIs à ne pas modifier.
- Dépendances interdites.

Validation
- Commandes à lancer.
- Tests à ajouter ou à faire passer.
- Captures UI si nécessaire.

Critères de refus
- Conditions où l’agent doit s’arrêter.

Sortie attendue
- PR, patch, analyse, test seul, spike, documentation, etc.
```

---

## 5.2 Les conventions deviendront essentielles

Les agents ont besoin de rails. Sans conventions explicites, ils produisent du code plausible mais souvent incohérent avec l’équipe ou le produit.

Les conventions devront couvrir :

- structure des dossiers ;
- nommage ;
- composants React ;
- hooks ;
- formulaires ;
- mutations GraphQL ;
- stratégie Apollo cache ;
- gestion des erreurs ;
- feature flags ;
- i18n ;
- tests unitaires ;
- tests Playwright ;
- accessibilité ;
- design system ;
- gestion des permissions ;
- observabilité ;
- performance.

Une convention importante doit idéalement être :

1. documentée ;
2. illustrée par un exemple canonique ;
3. vérifiée automatiquement si possible.

### Exemple

Au lieu de dire :

```text
Respecte les bonnes pratiques.
```

Il faudra dire :

```text
Pour une suppression :
- utiliser ConfirmDeletionModal ;
- suivre le pattern de DeleteDataPointButton.tsx ;
- utiliser les clés i18n sous metric_tag:: ;
- ne pas modifier le cache Apollo manuellement ;
- relancer TAGS_QUERY après suppression ;
- ajouter un test confirm/cancel.
```

---

## 5.3 Les tests deviendront l’interface de pilotage des agents

Les tests ne serviront plus seulement à éviter les régressions humaines. Ils deviendront une manière de dire aux agents ce qu’ils doivent réussir.

Flux recommandé :

```text
1. Écrire ou faire écrire un test de reproduction.
2. Demander à l’agent de faire passer ce test.
3. Interdire les modifications hors scope.
4. Lancer les tests existants.
5. Reviewer le changement.
```

Les tests les plus importants seront :

- tests de reproduction de bug ;
- tests unitaires sur logique pure ;
- tests de contrat API / GraphQL ;
- tests Playwright sur flux critiques ;
- tests de non-régression visuelle ;
- tests de performance ;
- tests d’accessibilité.

Une bonne équipe agent-ready est une équipe où l’agent peut échouer automatiquement quand il s’éloigne du comportement attendu.

---

## 5.4 Les fichiers d’instructions agents deviendront stratégiques

Les repos auront besoin de fichiers comme :

```text
AGENTS.md
copilot-instructions.md
docs/frontend-conventions.md
docs/testing.md
docs/architecture.md
docs/graphql-cache.md
docs/design-system.md
docs/i18n.md
```

Un bon `AGENTS.md` doit être court, opérationnel et maintenu.

### Exemple de structure

```md
# AGENTS.md

## Setup
- npm install
- npm run dev
- npm run typecheck

## Avant d’ouvrir une PR
- npm run typecheck
- npm test
- npm run lint

## Conventions frontend
- Utiliser les composants existants avant d’en créer de nouveaux.
- Ne pas ajouter de dépendance sans validation.
- Préférer les types GraphQL générés.
- Ne pas modifier le cache Apollo manuellement sauf cas documenté.

## Tests
- Pour un bugfix, ajouter un test de reproduction.
- Pour un flux utilisateur, préférer Playwright.
- Pour une logique pure, préférer un test unitaire.

## Interdits
- Ne pas modifier les fichiers générés.
- Ne pas changer le schema GraphQL.
- Ne pas modifier le design system depuis une feature locale.
```

---

## 5.5 Les outils de review devront évoluer

Plus les agents produiront de code, plus le risque sera d’avoir trop de PR à relire.

Il faudra des outils capables de répondre à des questions comme :

```text
Qu’est-ce qui a vraiment changé conceptuellement ?
Cette PR suit-elle nos conventions ?
Quels fichiers ont été modifiés hors scope ?
Quels tests prouvent le comportement ?
Cette abstraction existe-t-elle déjà ailleurs ?
Quels risques produit restent ouverts ?
```

Les PR devront probablement intégrer une section “AI trace”.

### Exemple

```md
## AI trace

Agent utilisé
- Codex / Claude Code / Copilot Agent / autre

Prompt initial
- ...

Scope demandé
- ...

Fichiers modifiés
- ...

Commandes lancées
- npm run typecheck
- npm test
- npm run lint

Résultats
- Typecheck : OK
- Tests unitaires : OK
- Playwright : non lancé

Incertitudes
- Non testé sur Safari.
- Pas de validation manuelle du mode legacy.
```

---

## 5.6 Les MCP et connexions outils devront être gouvernés

Les agents auront besoin d’accès à :

- GitHub / GitLab ;
- Jira / Linear ;
- Slack / Teams ;
- Google Drive / Notion ;
- Figma ;
- Datadog / Sentry ;
- bases de données de dev ;
- documentation interne ;
- CI/CD.

Mais ces accès créent des risques :

- fuite de secrets ;
- prompt injection depuis un ticket ou une issue ;
- outil trop permissif ;
- commande dangereuse ;
- accès à des données sensibles ;
- modification non souhaitée.

Il faudra donc :

- allowlist des MCP autorisés ;
- permissions par rôle ;
- environnements sandboxés ;
- logs d’audit ;
- séparation lecture / écriture ;
- secrets non exposés aux agents ;
- validation humaine avant actions sensibles ;
- politiques de sécurité explicites.

---

## 6. Feuille de route des travaux à mener

# Axe 1 — Préparer le codebase

## Objectif

Rendre le code plus lisible, plus testable et plus facilement manipulable par des agents.

## Travaux à mener

### 1. Cartographier les zones critiques

Identifier :

- modules complexes ;
- fichiers trop longs ;
- composants très couplés ;
- logique métier implicite ;
- zones sans tests ;
- flux utilisateurs critiques ;
- conventions non écrites.

### 2. Réduire les zones floues

Priorité aux endroits où les agents risquent de mal interpréter :

- permissions ;
- formulaires ;
- cache Apollo ;
- imports ;
- mapping tables ;
- i18n ;
- feature flags ;
- traitement des erreurs ;
- règles métier.

### 3. Extraire la logique testable

Favoriser :

- fonctions pures ;
- hooks simples ;
- composants moins massifs ;
- séparation logique métier / UI ;
- types explicites ;
- effets de bord isolés.

### 4. Améliorer les tests

Priorités :

- tests de reproduction pour bugs fréquents ;
- tests Playwright sur flux critiques ;
- tests unitaires sur logique métier ;
- tests de non-régression sur permissions ;
- tests de formulaires ;
- tests d’import ;
- tests de performance sur pages lourdes.

### 5. Créer des exemples canoniques

Documenter les meilleurs exemples existants :

- bon formulaire ;
- bonne table filtrable ;
- bonne mutation GraphQL ;
- bonne modal de confirmation ;
- bon usage de feature flag ;
- bon test Playwright ;
- bon traitement d’erreur ;
- bonne intégration i18n.

---

# Axe 2 — Normer les conventions

## Objectif

Transformer les conventions implicites en règles explicites, visibles et autant que possible automatisées.

## Travaux à mener

### 1. Écrire les conventions frontend

Créer ou mettre à jour :

```text
docs/frontend-conventions.md
docs/forms.md
docs/graphql.md
docs/testing.md
docs/i18n.md
docs/feature-flags.md
docs/design-system.md
```

### 2. Automatiser les règles importantes

Mettre en place ou renforcer :

- ESLint ;
- règles custom ;
- TypeScript strict ;
- Prettier ;
- tests obligatoires ;
- checks CI ;
- DangerJS ou équivalent ;
- lint sur imports interdits ;
- lint sur fichiers générés ;
- règles sur dépendances interdites.

### 3. Créer des ADR

Formaliser les décisions structurantes :

```text
ADR-001 : stratégie Apollo cache
ADR-002 : stratégie formulaires
ADR-003 : usage de Playwright vs tests unitaires
ADR-004 : stratégie i18n
ADR-005 : stratégie feature flags
ADR-006 : stratégie permissions frontend
ADR-007 : règles d’ajout de dépendances
```

### 4. Créer un glossaire métier

Les agents ont besoin de comprendre les termes du produit.

Créer un glossaire :

- Data Collection ;
- reporting period ;
- metric ;
- data point ;
- contributor ;
- quality control ;
- processing rule ;
- mapping table ;
- import answers ;
- previous answers ;
- permissions ;
- roles.

---

# Axe 3 — Préparer les outils

## Objectif

Construire progressivement un environnement où les agents peuvent travailler de manière contrôlée.

## Travaux à mener

### 1. Créer un premier `AGENTS.md`

Contenu minimal :

- setup ;
- commandes de test ;
- conventions critiques ;
- interdits ;
- procédure de PR ;
- zones sensibles ;
- exemples à suivre.

### 2. Définir les modes d’usage des agents

Exemples :

```text
Mode analyse
- L’agent lit et explique, mais ne modifie rien.

Mode test
- L’agent écrit uniquement un test de reproduction.

Mode patch
- L’agent corrige dans un scope limité.

Mode refactor
- L’agent peut restructurer, mais uniquement après validation humaine.

Mode spike
- L’agent explore une solution jetable sur une branche expérimentale.
```

### 3. Mettre en place des sandboxes

Prévoir :

- environnements isolés ;
- accès limités ;
- secrets protégés ;
- branches temporaires ;
- nettoyage automatique ;
- logs persistants ;
- CI déclenchable par agent.

### 4. Gouverner les MCP

Créer une politique :

```text
MCP autorisés
- GitHub lecture/écriture contrôlée
- Jira lecture
- Figma lecture
- Datadog lecture
- Google Drive lecture sur dossiers approuvés

MCP interdits ou restreints
- accès prod DB
- secrets
- outils de déploiement production
- messagerie externe en écriture
```

### 5. Standardiser les traces agentiques

Chaque PR générée ou modifiée significativement par agent doit contenir :

- agent utilisé ;
- prompt initial ;
- scope demandé ;
- fichiers touchés ;
- commandes lancées ;
- tests passés ;
- tests non lancés ;
- incertitudes ;
- risques ;
- validation humaine attendue.

---

# Axe 4 — Faire évoluer la gestion des tickets

## Objectif

Transformer les tickets en unités de travail pilotables par agents.

## Travaux à mener

### 1. Créer un template d’issue agent-compatible

Template recommandé :

```md
## Objectif

## Contexte

## Scope autorisé

## Scope interdit

## Contraintes techniques

## Référence / pattern à suivre

## Validation attendue

## Tests à ajouter ou modifier

## Critères de refus

## Sortie attendue
```

### 2. Découper les grosses issues

Règle générale :

```text
Une issue agent-friendly doit pouvoir être comprise, exécutée et reviewée indépendamment.
```

À éviter :

```text
Refondre toute la gestion des permissions.
```

À préférer :

```text
Ajouter un test de non-régression sur le bouton X selon la permission Y.
Extraire la condition dans une fonction pure.
Remplacer l’usage dans un composant.
Étendre progressivement.
```

### 3. Créer un rôle d’agent architecte

Avant exécution :

```text
Agent architecte :
- lit l’issue ;
- propose un découpage ;
- identifie les risques ;
- propose les fichiers probables ;
- recommande le niveau d’autonomie.
```

Puis validation humaine.

### 4. Ajouter une étape de comparaison

Pour les sujets complexes :

```text
Agent A : solution minimale.
Agent B : solution plus propre.
Agent C : spike long terme.
Agent reviewer : comparaison des trois.
Humain : arbitrage.
```

---

# Axe 5 — Former les développeurs

## Objectif

Faire évoluer les compétences des développeurs vers le pilotage, la validation et l’architecture de changements agentiques.

## Compétences à développer

### 1. Savoir découper

Un bon développeur devra savoir transformer :

```text
Grosse demande produit ambiguë
```

en :

```text
Sous-issues petites, testables, contrôlables et parallélisables.
```

### 2. Savoir écrire des contraintes

Compétence clé :

```text
Dire non à l’agent avant qu’il ne parte trop loin.
```

Exemples :

- ne modifie pas l’API ;
- ne touche pas au design system ;
- ne change pas les fichiers générés ;
- pas de nouvelle dépendance ;
- pas de refactor opportuniste ;
- ajoute un test avant le fix.

### 3. Savoir reviewer du code généré

La review doit vérifier :

- intention ;
- scope ;
- cohérence ;
- lisibilité ;
- conventions ;
- tests ;
- dette technique ;
- risque produit ;
- sur-abstraction ;
- compatibilité long terme.

### 4. Savoir écrire des tests de pilotage

Les développeurs devront écrire des tests non seulement pour vérifier le code, mais pour guider l’agent.

Exemple :

```text
Voici le test qui échoue.
Corrige le minimum pour le faire passer.
Ne modifie pas les snapshots sans justification.
```

### 5. Savoir reconnaître les limites de l’IA

Former les développeurs à repérer :

- hallucinations ;
- solutions trop larges ;
- abstraction inutile ;
- contournement ;
- tests faibles ;
- oubli des edge cases ;
- modification silencieuse de comportement ;
- dette cachée.

---

# Axe 6 — Adapter l’organisation

## Objectif

Préparer les pratiques d’équipe à une production de code plus rapide mais potentiellement plus bruyante.

## Travaux à mener

### 1. Définir une politique d’usage IA

Clarifier :

- ce qui est autorisé ;
- ce qui est interdit ;
- quand l’IA peut modifier du code ;
- quand une validation humaine est obligatoire ;
- quelles données peuvent être envoyées à quels outils ;
- quels outils sont approuvés ;
- comment tracer l’usage.

### 2. Adapter la Definition of Done

Ajouter :

```text
- scope respecté ;
- tests ajoutés ;
- commandes de validation lancées ;
- AI trace remplie si agent utilisé ;
- incertitudes listées ;
- absence de modification hors scope ;
- conventions respectées.
```

### 3. Mettre en place une review renforcée pour PR agentiques

Checklist spécifique :

```text
- Le ticket était-il assez précis ?
- La PR respecte-t-elle le scope ?
- L’agent a-t-il ajouté de la dette ?
- Les tests prouvent-ils vraiment le comportement ?
- La solution suit-elle un pattern existant ?
- Y a-t-il une abstraction inutile ?
- Les risques sont-ils documentés ?
```

### 4. Créer des boucles de retour

Mesurer régulièrement :

- temps gagné ;
- bugs introduits ;
- qualité des PR ;
- taux de rejet des PR agentiques ;
- temps de review ;
- zones où l’agent échoue souvent ;
- conventions manquantes ;
- tests manquants.

---

## 7. Priorités recommandées

# Priorité 1 — Court terme immédiat

À lancer rapidement :

1. Créer un `AGENTS.md` minimal.
2. Lister les conventions frontend implicites.
3. Créer un template d’issue agent-compatible.
4. Identifier 5 patterns canoniques à faire suivre aux agents.
5. Renforcer les tests sur les zones critiques.
6. Définir les règles de sécurité minimales pour outils IA/MCP.
7. Ajouter une section “AI trace” dans les PR concernées.

---

# Priorité 2 — Court terme

À lancer ensuite :

1. Documenter les conventions principales.
2. Automatiser les règles critiques avec lint/tests/CI.
3. Créer des ADR sur les décisions techniques importantes.
4. Mettre en place des modes d’usage des agents.
5. Former les développeurs au découpage d’issues.
6. Former les développeurs à la review de code généré.
7. Expérimenter les branches candidates sur un sujet non critique.

---

# Priorité 3 — Moyen terme

À préparer :

1. Construire un orchestrateur ou adopter une plateforme d’orchestration.
2. Standardiser les sandboxes cloud.
3. Brancher les agents aux logs, tickets, docs, design system et CI.
4. Créer un système de comparaison de branches candidates.
5. Mettre en place une gouvernance MCP complète.
6. Suivre des métriques qualité spécifiques aux agents.
7. Faire évoluer les rôles : développeur pilote, agent reviewer, agent architecte.

---

## 8. Risques à surveiller

## 8.1 Surproduction de code

Les agents peuvent produire beaucoup de code vite. Le risque est d’augmenter la dette plus vite que la capacité de review.

Réponse :

- petites issues ;
- petits scopes ;
- tests obligatoires ;
- PR courtes ;
- review renforcée.

## 8.2 Cohérence globale dégradée

Chaque agent peut produire une solution correcte localement mais incohérente avec le système global.

Réponse :

- conventions fortes ;
- exemples canoniques ;
- architecture documentée ;
- agent reviewer ;
- validation humaine.

## 8.3 Fausse confiance

Une PR générée par agent peut être propre, typée et testée, mais résoudre le mauvais problème.

Réponse :

- tests comportementaux ;
- validation produit ;
- résumé des risques ;
- comparaison avec le ticket initial.

## 8.4 Sécurité

Les agents connectés aux outils internes peuvent lire ou modifier des informations sensibles.

Réponse :

- accès lecture seule par défaut ;
- allowlist MCP ;
- sandbox ;
- audit logs ;
- séparation des permissions ;
- validation humaine pour actions sensibles.

## 8.5 Déqualification des juniors

Si les juniors délèguent trop tôt, ils risquent de produire sans comprendre.

Réponse :

- obligation d’expliquer les PR ;
- exercices manuels ;
- pair review ;
- formation aux fondamentaux ;
- usage encadré de l’IA.

---

## 9. Vision cible

L’entreprise doit viser une organisation où :

```text
Les agents peuvent produire vite,
mais uniquement dans un cadre clair,
testable, observable et gouverné.
```

Le bon modèle n’est pas :

```text
Donner le codebase entier à un agent et espérer une bonne PR.
```

Le bon modèle est :

```text
Découper le travail,
contraindre le scope,
fournir le contexte,
lancer plusieurs hypothèses,
tester automatiquement,
comparer les résultats,
faire arbitrer par un humain compétent.
```

---

## 10. Formule de synthèse

Le futur proche du développement logiciel ne sera pas simplement plus automatisé.

Il sera plus **orchestré**.

Les entreprises qui réussiront seront celles qui traiteront leurs conventions, leurs tests, leur documentation, leur architecture et leur outillage comme une infrastructure destinée à la fois aux humains et aux agents.

```text
Meilleurs agents + mauvais contexte = bruit rapide.

Bons agents + bon contexte + bons tests + bonnes conventions = accélération maîtrisée.
```
