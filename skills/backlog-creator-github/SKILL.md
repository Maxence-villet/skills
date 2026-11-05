# Skill: GitHub Backlog Creator

## 1. Description

Ce skill permet à un agent IA de transformer des fichiers et documents fournis par l'utilisateur en un backlog de User Stories directement dans GitHub Projects.

L'agent agit comme un Product Owner expérimenté.

Il doit analyser les sources, comprendre les fonctionnalités attendues, identifier les besoins utilisateurs et les transformer en User Stories structurées, priorisées et exploitables par une équipe de développement.

Toutes les opérations GitHub doivent être réalisées à l'aide de GitHub CLI (`gh`) et, lorsque nécessaire, de son API GraphQL.

## 2. Objectif

À partir des fichiers fournis par l'utilisateur, l'agent doit :

1. Analyser et comprendre le projet.
2. Identifier les fonctionnalités attendues.
3. Identifier les différents rôles utilisateurs.
4. Transformer les fonctionnalités en User Stories.
5. Définir les critères d'acceptation de chaque User Story.
6. Identifier les dépendances entre User Stories.
7. Prioriser les User Stories.
8. Identifier les éventuelles fonctionnalités déjà développées.
9. Vérifier le backlog GitHub existant.
10. Créer les User Stories dans GitHub Issues.
11. Ajouter les issues dans le GitHub Project Kanban.

Le résultat doit être un backlog clair, cohérent, non redondant et directement exploitable.

---

## 3. Convention obligatoire des User Stories

Toutes les User Stories doivent respecter strictement le format suivant.

### Format du titre

```text
US-[NUMÉRO] - [Titre de la fonctionnalité]
```

Exemples :

```text
US-001 - Authentification utilisateur
US-002 - Réinitialisation du mot de passe
US-003 - Gestion du profil utilisateur
US-004 - Export des données
```

Les numéros doivent être uniques et séquentiels.

Avant de créer une nouvelle User Story, l'agent doit récupérer les numéros déjà utilisés dans le backlog GitHub afin d'éviter les doublons.

Ne jamais réattribuer un numéro déjà utilisé, même si une ancienne User Story est fermée.

### Format de la description

Chaque User Story doit obligatoirement respecter cette structure :

```markdown
# US-001 - Authentification utilisateur

**En tant que** utilisateur

**Je veux** pouvoir me connecter à l'application avec mon adresse email et mon mot de passe

**Afin de** accéder aux fonctionnalités correspondant à mon compte.

## Critères d'acceptation

- L'utilisateur peut saisir son adresse email.
- L'utilisateur peut saisir son mot de passe.
- L'utilisateur peut se connecter avec des identifiants valides.
- Un message d'erreur est affiché lorsque les identifiants sont incorrects.
- L'utilisateur est redirigé vers son espace personnel après une connexion réussie.
```

### Règles de rédaction

**En tant que :**

Identifier le rôle qui bénéficie de la fonctionnalité.

Exemples :
- Visiteur
- Utilisateur
- Administrateur
- Gestionnaire
- Développeur
- Responsable métier
- Super administrateur

Ne pas utiliser un rôle technique si le besoin concerne un utilisateur métier.

**Je veux :**

Décrire précisément l'action ou la fonctionnalité attendue.

La phrase doit exprimer un besoin fonctionnel et non une solution technique imposée.

Exemple :

À privilégier :
> Je veux pouvoir filtrer les résultats par date.

À éviter :
> Je veux qu'un endpoint REST avec un paramètre date soit développé.

**Afin de :**

Expliquer la finalité et la valeur apportée par la fonctionnalité.

La finalité doit être compréhensible et orientée utilisateur.

### Critères d'acceptation

Chaque User Story doit posséder une section `Critères d'acceptation`.

Les critères doivent :

- Être rédigés sous forme de listes.
- Décrire des comportements observables.
- Être vérifiables et testables.
- Être suffisamment précis pour éviter les ambiguïtés.
- Couvrir les cas nominaux.
- Couvrir les cas d'erreur pertinents.
- Couvrir les règles métier.
- Couvrir les cas limites lorsqu'ils sont identifiés.

Exemple :

```markdown
## Critères d'acceptation

- L'utilisateur peut accéder à la page de connexion.
- Les champs email et mot de passe sont obligatoires.
- Une adresse email invalide déclenche un message d'erreur.
- Un utilisateur avec des identifiants incorrects ne peut pas accéder à l'application.
- Après trois tentatives échouées, le mécanisme de protection prévu est appliqué.
- Une connexion réussie redirige l'utilisateur vers son tableau de bord.
```

Ne pas créer de critères artificiels uniquement pour augmenter leur nombre.

Un critère doit correspondre à un comportement attendu identifiable dans les sources ou à une règle métier nécessaire et explicitement validée.

---

## 4. Analyse des fichiers sources

L'agent doit analyser tous les fichiers fournis par l'utilisateur.

Les sources peuvent être :

- Cahier des charges.
- Documentation fonctionnelle.
- Documentation technique.
- README.
- Spécifications API.
- Fichiers OpenAPI / Swagger.
- Diagrammes d'architecture.
- Maquettes UX/UI.
- Fichiers de configuration.
- Code source.
- Documents PDF, Word ou Markdown.

### Méthode d'analyse

Pour chaque source :

1. Identifier les objectifs du document.
2. Identifier les fonctionnalités décrites.
3. Identifier les utilisateurs concernés.
4. Identifier les règles métier.
5. Identifier les contraintes fonctionnelles.
6. Identifier les interactions entre fonctionnalités.
7. Identifier les cas d'utilisation.
8. Identifier les comportements attendus.
9. Identifier les cas d'erreur.
10. Identifier les fonctionnalités déjà implémentées.

L'agent doit regrouper les informations provenant de plusieurs fichiers lorsqu'elles décrivent une même fonctionnalité.

Une fonctionnalité décrite dans plusieurs documents ne doit pas générer plusieurs User Stories.

### Gestion des informations manquantes

L'agent doit distinguer :

- Les exigences explicites.
- Les déductions raisonnables.
- Les hypothèses.
- Les informations manquantes.

Il ne doit jamais inventer une règle métier.

Lorsqu'une information est nécessaire à la rédaction d'une User Story, mais absente des sources, l'agent doit signaler le point à clarifier.

Il peut proposer une hypothèse, mais doit la présenter comme telle et attendre sa validation avant de l'intégrer au backlog définitif.

---

## 5. Construction du backlog

### Identification des fonctionnalités

L'agent doit identifier les fonctionnalités indépendantes du projet.

Une fonctionnalité doit correspondre à une capacité identifiable apportant une valeur à un utilisateur.

Exemples :

- Authentification.
- Gestion des utilisateurs.
- Gestion des permissions.
- Consultation des données.
- Modification des données.
- Export des données.
- Recherche et filtrage.
- Notifications.
- Administration.

### Découpage des fonctionnalités

Une fonctionnalité peut générer plusieurs User Stories lorsqu'elle couvre plusieurs besoins utilisateurs indépendants.

Exemple pour la gestion des utilisateurs :

```text
US-001 - Créer un utilisateur
US-002 - Consulter un utilisateur
US-003 - Modifier un utilisateur
US-004 - Désactiver un utilisateur
US-005 - Supprimer un utilisateur
```

Ne pas créer une User Story par écran, composant technique ou endpoint API.

Une User Story doit représenter un besoin fonctionnel cohérent.

### Granularité

Chaque User Story doit être suffisamment petite pour pouvoir être :

- Compris par l'équipe.
- Estimé.
- Développé.
- Testé.
- Validé indépendamment.

Si une User Story est trop importante, la découper en plusieurs User Stories tout en conservant une cohérence fonctionnelle.

Éviter les User Stories trop techniques ou trop générales.

---

## 6. Priorisation du backlog

Chaque User Story doit recevoir une priorité.

Utiliser les niveaux suivants :

| Priorité | Description |
|---|---|
| P0 - Critical | Indispensable au fonctionnement ou bloquante |
| P1 - High | Forte valeur métier ou fonctionnalité majeure |
| P2 - Medium | Fonctionnalité importante mais non bloquante |
| P3 - Low | Amélioration ou fonctionnalité secondaire |

La priorisation doit prendre en compte :

- La valeur métier.
- Les besoins utilisateurs.
- Les dépendances.
- Les risques.
- Les contraintes du projet.
- L'ordre logique de mise en œuvre.

Ne jamais attribuer arbitrairement une priorité P0.

Si la priorité ne peut pas être déterminée, le signaler à l'utilisateur.

---

## 7. Métadonnées des User Stories

Chaque User Story doit être associée aux métadonnées suivantes dans GitHub Projects.

| Champ | Type | Valeurs |
|---|---|---|
| Status | Single Select | Backlog, Ready, In Progress, In Review, Done |
| Priority | Single Select | P0, P1, P2, P3 |
| Effort | Number | Estimation en points, si connue |

Les nouveaux éléments doivent être créés avec le statut `Backlog`.

L'estimation de l'effort est facultative. Ne jamais inventer une estimation.

Si le champ n'existe pas, ne pas le créer ou modifier sans validation préalable.

---

## 8. Gestion du GitHub Project Kanban

Le backlog doit être centralisé dans un GitHub Project de type Kanban.

Colonnes recommandées :

- Backlog
- Ready
- In Progress
- In Review
- Done

Toutes les nouvelles User Stories doivent être ajoutées à la colonne `Backlog`.

L'agent doit vérifier que le Project existe avant toute création.

Si aucun Project n'existe, demander à l'utilisateur s'il souhaite en créer un.

Ne jamais créer un nouveau Project si un Project cible existe déjà.

---

## 9. Utilisation de GitHub CLI

L'agent doit utiliser GitHub CLI pour toutes les opérations GitHub.

### Vérifier l'environnement

```bash
gh --version
gh auth status
gh repo view --json name,owner,url
```

### Identifier le repository

```bash
gh repo view --json name,owner,url
```

### Lister les GitHub Projects

```bash
gh project list --owner OWNER
```

### Lister les issues existantes

```bash
gh issue list \
  --repo OWNER/REPO \
  --state all \
  --limit 1000
```

Pour récupérer les numéros, titres et descriptions, utiliser les options JSON :

```bash
gh issue list \
  --repo OWNER/REPO \
  --state all \
  --limit 1000 \
  --json number,title,body,state,labels,url
```

### Créer une User Story

Préparer le contenu dans un fichier Markdown temporaire.

Exemple :

```bash
gh issue create \
  --repo OWNER/REPO \
  --title "US-001 - Authentification utilisateur" \
  --body-file story.md \
  --label "user-story"
```

Le label `user-story` doit exister avant son utilisation.

### Vérifier les labels

```bash
gh label list --repo OWNER/REPO
```

### Ajouter une issue au GitHub Project

```bash
gh project item-add PROJECT_NUMBER \
  --owner OWNER \
  --url ISSUE_URL
```

### Lister les éléments du Project

```bash
gh project item-list PROJECT_NUMBER \
  --owner OWNER \
  --format json
```

### Modifier les champs du Project

Lorsque nécessaire, utiliser GraphQL via :

```bash
gh api graphql -f query='...'
```

L'agent doit récupérer dynamiquement les identifiants du Project, des items, des champs et des options avant toute modification.

Ne jamais inventer un identifiant GraphQL.

Pour connaître les options et les commandes disponibles :

```bash
gh project --help
gh issue --help
gh api graphql --help
```

---

## 10. Gestion des numéros US

La numérotation est globale au repository et ne dépend pas du statut de l'issue.

Avant de créer de nouvelles User Stories :

1. Récupérer toutes les issues existantes.
2. Identifier les titres commençant par `US-`.
3. Extraire les numéros.
4. Identifier le numéro le plus élevé.
5. Affecter les numéros suivants aux nouvelles User Stories.

Exemple :

Issues existantes :

```text
US-001 - Authentification
US-002 - Gestion du profil
US-003 - Consultation des données
```

Nouvelles User Stories :

```text
US-004 - Modification des données
US-005 - Export des données
US-006 - Recherche des données
```

Ne jamais réutiliser un numéro existant.

Si plusieurs agents travaillent simultanément sur le même backlog, vérifier à nouveau les numéros juste avant chaque création afin d'éviter les collisions.

---

## 11. Détection des doublons

Avant de créer une User Story :

1. Comparer son titre aux User Stories existantes.
2. Comparer son besoin fonctionnel.
3. Comparer son rôle utilisateur.
4. Comparer ses critères d'acceptation.
5. Vérifier si une issue existante couvre déjà ce besoin.

Si une User Story similaire existe :

- Ne pas créer de doublon.
- Signaler l'issue existante.
- Proposer éventuellement de compléter son contenu.
- Attendre l'autorisation avant toute modification.

L'agent doit également identifier les User Stories déjà implémentées dans le code source, sans considérer automatiquement qu'une fonctionnalité présente dans le code est terminée ou validée.

---

## 12. Traçabilité des sources

Chaque User Story doit comporter une section permettant de retrouver l'origine du besoin.

Ajouter à la fin de la description :

```markdown
## Sources

- docs/authentication.md — Section 2.1
- docs/api.yaml — Endpoint /auth/login
```

Les références doivent être précises lorsque les informations sont disponibles.

Ne jamais inventer un nom de fichier, une section ou une référence.

Si aucune source précise n'est identifiable, le signaler.

---

## 13. Workflow obligatoire

L'agent doit travailler en deux phases distinctes.

### PHASE 1 — PLAN

Cette phase ne doit effectuer aucune modification sur GitHub.

Actions :

1. Analyser les fichiers.
2. Identifier les fonctionnalités.
3. Identifier les rôles utilisateurs.
4. Construire les User Stories.
5. Rédiger les critères d'acceptation.
6. Identifier les dépendances.
7. Définir les priorités proposées.
8. Vérifier les issues existantes.
9. Détecter les doublons.
10. Affecter les futurs numéros US.
11. Présenter le backlog à l'utilisateur.

Format de présentation :

```markdown
# Proposition de backlog

## Synthèse

- Nombre de fonctionnalités : X
- Nombre de User Stories : X
- Nombre de doublons détectés : X
- Nombre de points à clarifier : X

## User Stories proposées

### US-001 - Authentification utilisateur

**En tant que** utilisateur

**Je veux** pouvoir me connecter à l'application

**Afin de** accéder à mon espace personnel.

## Critères d'acceptation

- L'utilisateur peut saisir son email.
- L'utilisateur peut saisir son mot de passe.
- Les identifiants invalides déclenchent un message d'erreur.

**Priorité :** P1

**Dépendances :** Aucune

**Sources :**
- docs/authentication.md

---

### US-002 - Réinitialisation du mot de passe

...
```

Terminer cette phase en demandant explicitement :

"Valides-tu cette proposition de backlog pour que je puisse créer les User Stories dans GitHub Projects ?"

Aucune écriture GitHub ne doit être effectuée sans validation.

### PHASE 2 — APPLY

Cette phase commence uniquement après validation explicite de l'utilisateur.

Actions :

1. Vérifier le repository.
2. Vérifier le GitHub Project.
3. Récupérer les dernières issues existantes.
4. Vérifier les numéros US.
5. Vérifier les doublons une dernière fois.
6. Créer les issues validées.
7. Ajouter les issues au GitHub Project.
8. Renseigner les champs Priority et Status.
9. Vérifier les résultats.
10. Générer un rapport final.

Ne pas créer les User Stories qui n'ont pas été validées.

---

## 14. Idempotence et mise à jour du backlog

Le skill doit être idempotent.

Si l'utilisateur fournit à nouveau les mêmes fichiers :

- Ne pas recréer les User Stories existantes.
- Identifier les nouvelles fonctionnalités.
- Identifier les modifications des besoins.
- Identifier les critères d'acceptation qui ont évolué.
- Signaler les User Stories devenues obsolètes.
- Proposer les mises à jour nécessaires.

Ne jamais modifier automatiquement une issue existante.

Toute modification doit être présentée à l'utilisateur et validée avant application.

---

## 15. Rapport final

Après la création des User Stories, produire un rapport Markdown.

```markdown
# Rapport de création du backlog

## GitHub Project

- Repository : OWNER/REPO
- Project : Nom du Project
- URL : URL du Project

## User Stories créées

| ID | Titre | Priorité | Status | URL |
|---|---|---|---|---|
| US-001 | Authentification utilisateur | P1 | Backlog | URL |
| US-002 | Réinitialisation du mot de passe | P2 | Backlog | URL |

## Résumé

- User Stories proposées : X
- User Stories créées : X
- User Stories existantes réutilisées : X
- Doublons évités : X
- Points à clarifier : X

## Points d'attention

- Point 1
- Point 2
```

---

## 16. Règles fondamentales

L'agent doit toujours respecter les principes suivants :

1. Une fonctionnalité utilisateur = une ou plusieurs User Stories cohérentes.
2. Chaque User Story respecte strictement le format `US-[NUMÉRO] - [TITRE]`.
3. Chaque User Story contient obligatoirement :
   - En tant que
   - Je veux
   - Afin de
   - Critères d'acceptation
4. Les critères d'acceptation doivent être testables.
5. Les User Stories doivent être compréhensibles par une personne métier et par un développeur.
6. Les besoins techniques ne doivent pas remplacer les besoins utilisateurs.
7. Les hypothèses doivent être identifiées.
8. Les doublons doivent être évités.
9. Les numéros US doivent rester uniques.
10. Les sources doivent être traçables.
11. Le backlog doit être présenté avant toute écriture GitHub.
12. Aucune issue existante ne doit être modifiée sans validation.

**Résultat attendu : un backlog GitHub Projects composé de User Stories homogènes, compréhensibles, priorisées, testables et directement exploitables par l'équipe de développement.**
