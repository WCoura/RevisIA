
# RévisIA — Assistant de révision intelligent

## 1. Présentation du projet

RévisIA est une application mobile Android conçue pour aider les étudiants à réviser leurs cours à l'aide de l'intelligence artificielle.

L'utilisateur saisit un texte de cours et choisit un mode de révision. L'application communique avec un serveur Node.js, qui utilise un modèle d'intelligence artificielle local pour générer une fiche de révision en français.

L'objectif est de faciliter la compréhension, la mémorisation et la préparation aux évaluations.

## 2. Membres de l'équipe et répartition des tâches

Le projet RévisIA a été réalisé en équipe de trois étudiants. Les responsabilités ont été réparties entre le développement de l'application mobile, l'intégration de l'intelligence artificielle et la validation du fonctionnement global.

### Wallace Coura — Développement de l'application mobile

- Création et configuration du projet NativeScript Vue.
- Développement de l'interface utilisateur dans `Home.vue`.
- Implémentation des trois modes de révision : Résumé, Questions et Résumé + Questions.
- Gestion des champs de saisie, des boutons et de l'affichage des résultats.
- Mise en place des messages de validation et de chargement.

### Adryen Moser — Développement du serveur et intégration de l'IA

- Création du serveur Node.js avec Express.
- Développement de l'API REST et de la route `/revision`.
- Configuration de l'environnement Ollama et du modèle Llama 3.2.
- Élaboration des consignes pédagogiques envoyées à l'intelligence artificielle.
- Implémentation des mécanismes de validation et de filtrage des réponses générées.

### Abdelwahhab Bouziane — Intégration, tests et documentation

- Configuration et utilisation de l'émulateur Android.
- Vérification de la communication entre l'application mobile et le serveur.
- Réalisation des tests fonctionnels des trois modes de révision.
- Vérification des résultats générés et identification des problèmes.
- Préparation de la documentation technique dans le `README.md`.
- Participation à la préparation de la démonstration vidéo et du dépôt GitHub.

### Collaboration de l'équipe

Les trois membres ont participé à la conception du projet, aux discussions techniques, à la résolution des problèmes et à la validation finale de l'application.

## 3. Technologies utilisées

| Technologie | Utilisation |
|---|---|
| NativeScript | Développement de l'application Android |
| Vue.js | Interface utilisateur |
| JavaScript | Logique de l'application et du serveur |
| Node.js | Exécution du serveur |
| Express.js | Création de l'API REST |
| Ollama | Exécution locale du modèle d'IA |
| Llama 3.2 | Génération des fiches de révision |
| Android Studio | Émulateur Android |
| Visual Studio Code | Développement et tests |
| Git et GitHub | Gestion et partage du code source |

## 4. Fonctionnalités

### 4.1 Résumé

L'application génère un résumé du texte fourni et présente les points essentiels.

### 4.2 Questions de compréhension

L'application génère jusqu'à cinq questions de compréhension accompagnées de leurs réponses.

Le nombre de questions dépend des informations disponibles dans le texte.

### 4.3 Résumé + Questions

Ce mode combine :
- Un résumé du texte.
- Les points essentiels.
- Des questions de compréhension et leurs réponses.

### 4.4 Validation des données

L'application vérifie que :
- Le texte n'est pas vide.
- Le texte ne dépasse pas 10 000 caractères.
- Le mode de révision est valide.

### 4.5 Vérification des réponses

Le serveur applique des règles pour limiter l'ajout d'informations absentes du texte.

Un mécanisme de filtrage élimine certaines questions incomplètes ou contenant des réponses insuffisantes.

Cependant, les réponses générées par l'intelligence artificielle doivent toujours être vérifiées par l'utilisateur.

## 5. Architecture du projet

L'application repose sur trois composants :

1. Application mobile NativeScript Vue.
2. Serveur REST Node.js avec Express.
3. Modèle d'intelligence artificielle Llama 3.2 exécuté localement avec Ollama.

### Fonctionnement

1. L'utilisateur saisit son texte dans l'application Android.
2. Il sélectionne un mode de révision.
3. L'application envoie une requête HTTP POST au serveur.
4. Le serveur valide les données.
5. Le serveur transmet une consigne au modèle Llama 3.2.
6. L'IA génère une réponse.
7. Le serveur traite et retourne le résultat.
8. L'application affiche la fiche de révision.

## 6. Structure du projet

```text
RevisIA/
├── MobileIA/
│   ├── app/
│   │   └── components/
│   │       └── Home.vue
│   ├── App_Resources/
│   ├── package.json
│   └── nativescript.config.ts
│
├── servidor/
│   ├── server.js
│   ├── package.json
│   └── package-lock.json
│
├── .gitignore
└── README.md
```

Les dossiers générés automatiquement, notamment `node_modules` et `platforms`, ne sont pas inclus dans le dépôt Git.

## 7. Prérequis

Pour exécuter le projet, installer :

- Node.js et npm.
- NativeScript CLI.
- Android Studio et un émulateur Android configuré.
- Java JDK compatible avec NativeScript.
- Ollama.
- Le modèle Llama 3.2.

Le serveur et Ollama doivent être accessibles depuis le même ordinateur.

## 8. Installation

### 8.1 Télécharger le projet

Cloner le dépôt GitHub :

```bash
git clone URL_DU_DEPOT
cd RevisIA
```

Remplacer `URL_DU_DEPOT` par l'adresse du dépôt GitHub.

### 8.2 Installer les dépendances du serveur

Dans un terminal :

```bash
cd servidor
npm install
```

### 8.3 Installer les dépendances mobiles

Dans un autre terminal :

```bash
cd MobileIA
npm install
```

### 8.4 Installer le modèle d'intelligence artificielle

```bash
ollama pull llama3.2
```

Vérifier l'installation :

```bash
ollama list
```

## 9. Exécution

### 9.1 Démarrer Ollama

S'assurer que le service Ollama fonctionne.

Si nécessaire, démarrer le service avec :

```bash
ollama serve
```

Si Ollama est déjà actif, il n'est pas nécessaire de lancer une deuxième instance.

### 9.2 Démarrer le serveur Node.js

Depuis le dossier `servidor` :

```bash
node server.js
```

Message attendu :

```text
Serveur RévisIA actif sur le port 3000
```

Le serveur est accessible à l'adresse :

```text
http://localhost:3000
```

### 9.3 Démarrer l'application Android

Depuis le dossier `MobileIA` :

```bash
ns run android
```

L'émulateur Android doit être configuré et démarré.

Dans l'émulateur Android, l'application communique avec le serveur local à l'adresse :

```text
http://10.0.2.2:3000/revision
```

Cette adresse est spécifique à l'émulateur Android et permet d'accéder au serveur exécuté sur l'ordinateur.

## 10. API REST

### POST /revision

Cette route reçoit un texte et le type de révision souhaité.

Exemple de requête :

```json
{
  "texte": "La Terre tourne autour du Soleil.",
  "typeRevision": "resume"
}
```

Modes disponibles :

- `resume`
- `questions`
- `complet`

Exemple de réponse :

```json
{
  "resultat": "RÉSUMÉ\nLa Terre tourne autour du Soleil."
}
```

Le serveur retourne des messages d'erreur lorsque les données sont invalides ou lorsque le service d'IA est indisponible.

## 11. Tests réalisés

Les fonctionnalités suivantes ont été testées sur un émulateur Android :

| Test | Résultat |
|---|---|
| Génération d'un résumé | Réussi |
| Génération de questions | Réussi |
| Génération d'un résumé avec questions | Réussi |
| Validation d'un texte vide | Réussi |
| Gestion d'une erreur de connexion | Réussi |
| Texte court sur la Terre | Réussi |
| Texte sur la photosynthèse | Réussi |
| Filtrage de certaines questions insuffisantes | Réussi |

Les tests ont permis d'améliorer les consignes du modèle et le traitement des questions générées.

## 12. Limites du projet

- Le modèle d'IA peut produire des réponses inexactes.
- Le filtrage ne garantit pas l'exactitude de toutes les informations.
- La génération dépend des performances de l'ordinateur.
- Ollama et le serveur doivent fonctionner pour utiliser l'application.
- L'adresse du serveur est actuellement configurée pour un émulateur Android local.
- La durée de génération peut varier selon la longueur du texte.

## 13. Améliorations possibles

- Ajouter un historique des révisions.
- Permettre l'enregistrement des fiches.
- Exporter les résultats en PDF.
- Ajouter davantage de modèles d'intelligence artificielle.
- Améliorer la vérification des réponses générées.
- Permettre la configuration de l'adresse du serveur.

## 14. Démonstration vidéo

Une démonstration du projet sera enregistrée avec OBS Studio.

Elle présentera :
- L'interface de l'application.
- Les trois modes de révision.
- La communication avec le serveur Node.js.
- L'utilisation locale de Llama 3.2.
- Les tests de fonctionnement.

Lien de la vidéo : à ajouter.

## 15. Conclusion

RévisIA démontre l'intégration d'une application mobile multiplateforme avec une API REST et un modèle d'intelligence artificielle local.

Le projet permet de transformer un texte de cours en matériel de révision et illustre l'utilisation de NativeScript Vue, Node.js, Express et Ollama dans une application pédagogique.
