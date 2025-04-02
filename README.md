# n8n Workflow : Automatisation de la Création de Dépôts GitHub

Un workflow n8n pour créer automatiquement de nouveaux dépôts sur GitHub, simplifiant la mise en place de nouveaux projets et assurant la cohérence des configurations initiales.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT) ## Table des Matières

* [Description](#description)
* [Fonctionnalités](#fonctionnalités)
* [Le Fichier JSON du Workflow](#le-fichier-json-du-workflow)
* [Prérequis](#prérequis)
* [Installation et Configuration](#installation-et-configuration)
* [Utilisation](#utilisation)
* [Personnalisation](#personnalisation)
* [Contribution](#contribution)
* [Licence](#licence)

## Description

Ce projet fournit un fichier JSON représentant un workflow n8n. Ce workflow permet de créer de nouveaux dépôts GitHub directement depuis n8n, que ce soit manuellement ou en réponse à d'autres événements dans vos automatisations. L'objectif est de gagner du temps et de standardiser la création de dépôts.

## Fonctionnalités

* Création de nouveaux dépôts GitHub.
* Définition du nom et de la description du dépôt (statiquement ou dynamiquement).
* Choix de la visibilité du dépôt (privé par défaut).
* Initialisation optionnelle du dépôt avec un fichier `README.md`.
* Utilise l'authentification OAuth2 (recommandé) ou par Token d'accès personnel GitHub via les identifiants n8n.

## Le Fichier JSON du Workflow

Le cœur de ce projet est le fichier JSON ci-dessous. Vous devez l'importer dans votre instance n8n.

**(Rappel : Choisissez un nom de fichier pertinent lorsque vous enregistrez ce contenu, par exemple `n8n_github_create_repo.json`)**

```json
{
  "name": "Création Dépôt GitHub Automatisée",
  "nodes": [
    {
      "parameters": {},
      "name": "Start",
      "type": "n8n-nodes-base.start",
      "typeVersion": 1,
      "position": [
        250,
        300
      ],
      "id": "5f8f5a7f-0e8a-4b4a-8a5d-1e2f3c4d5e6f"
    },
    {
      "parameters": {
        "authentication": "oAuth2",
        "resource": "repository",
        "operation": "create",
        "repository": "={{ $json.repoName || 'nouveau-depot-test-n8n' }}",
        "description": "={{ $json.repoDescription || 'Dépôt créé automatiquement via n8n' }}",
        "options": {
          "private": true,
          "autoInit": true
        }
      },
      "name": "Créer Dépôt GitHub",
      "type": "n8n-nodes-base.github",
      "typeVersion": 4, // Vérifiez si cette version correspond à votre n8n
      "position": [
        450,
        300
      ],
      "id": "a1b2c3d4-e5f6-7890-1234-abcdefghijkl",
      "credentials": {
        "githubOAuth2Api": {
          "id": "YOUR_CREDENTIAL_ID", // <-- À REMPLACER !
          "name": "GitHub OAuth2 API" // Nom de vos identifiants dans n8n
        }
      }
    }
  ],
  "connections": {
    "Start": {
      "main": [
        [
          {
            "node": "Créer Dépôt GitHub",
            "type": "main",
            "index": 0
          }
        ]
      ]
    }
  },
  "settings": {},
  "staticData": null,
  "pinData": {},
  "versionId": "some-unique-version-id", // Généré par n8n
  "triggerCount": 0,
  "tags": [
    {
      "id": "tag-github",
      "name": "GitHub"
    },
    {
      "id": "tag-automation",
      "name": "Automatisation"
    }
  ]
}
```

## Prérequis

* Une instance de [n8n](https://n8n.io/) fonctionnelle (auto-hébergée ou cloud).
* Un compte GitHub avec les permissions nécessaires pour créer des dépôts.

## Installation et Configuration

Suivez ces étapes pour mettre en place le workflow :

1.  **Obtenir le Fichier JSON :**
    * Clonez ce dépôt : `git clone <URL_DU_REPO>` (Si ce README fait partie d'un dépôt)
    * OU téléchargez directement le fichier `.json` s'il est fourni séparément.
    * OU copiez le contenu JSON de la section [Le Fichier JSON du Workflow](#le-fichier-json-du-workflow).

2.  **Configurer les Identifiants GitHub dans n8n :**
    * Dans votre instance n8n, allez dans `Credentials` -> `Add credential`.
    * Sélectionnez `GitHub OAuth2 API` (recommandé) ou `GitHub API` (pour utiliser un Personal Access Token).
    * Suivez les instructions pour connecter votre compte GitHub. Assurez-vous que l'application OAuth ou le token a bien les permissions `repo` (ou `public_repo` si vous ne créez que des dépôts publics).
    * **Notez précieusement l'ID** de cet identifiant une fois créé (visible dans l'URL en mode édition) et le nom que vous lui avez donné.

3.  **Modifier le Fichier JSON (si nécessaire) :**
    * Si vous avez copié le texte JSON, collez-le dans un éditeur. Si vous avez téléchargé/cloné le fichier, ouvrez-le.
    * Localisez la section `credentials` dans le nœud "Créer Dépôt GitHub".
    * **Remplacez `"YOUR_CREDENTIAL_ID"` par l'ID réel de vos identifiants GitHub n8n (obtenu à l'étape 2).** C'est une étape cruciale.
    * Vérifiez que le `name` correspond bien au nom de vos identifiants dans n8n (par défaut, souvent "GitHub OAuth2 API").
    * Enregistrez le fichier avec une extension `.json` (ex: `n8n_github_create_repo.json`) si vous avez modifié le contenu copié.

4.  **Importer le Workflow dans n8n :**
    * Dans n8n, allez dans `Workflows`.
    * Cliquez sur `Add Workflow` -> `Import from file / JSON`.
    * Collez le JSON modifié ou sélectionnez le fichier `.json` mis à jour.
    * Cliquez sur `Import`. Le workflow nommé "Création Dépôt GitHub Automatisée" devrait apparaître.

## Utilisation

1.  **Activation :** Assurez-vous que le workflow est activé ("Active" toggle ON) dans n8n si vous souhaitez l'utiliser avec des déclencheurs automatiques. Pour un test manuel, cela n'est pas nécessaire.
2.  **Déclenchement Manuel :**
    * Ouvrez le workflow dans l'éditeur n8n.
    * Cliquez sur `Execute Workflow`.
    * Le workflow utilisera les valeurs par défaut (`nouveau-depot-test-n8n` comme nom, etc.) définies dans le nœud GitHub, ou les valeurs fournies par `$json` si vous avez modifié le déclencheur "Start" pour envoyer des données de test.
3.  **Déclenchement Dynamique :**
    * Remplacez le nœud "Start" par un autre déclencheur (ex: `Webhook`, `Schedule`, `Form Trigger`, un autre service...).
    * Assurez-vous que les données entrantes dans le nœud GitHub contiennent les clés `repoName` et/ou `repoDescription` si vous souhaitez définir ces valeurs dynamiquement. L'expression `={{ $json.KEY || 'DEFAULT' }}` gère cela.

## Personnalisation

* **Valeurs par Défaut :** Modifiez les chaînes de caractères après l'opérateur `||` dans les champs `repository` et `description` du nœud GitHub pour changer les noms et descriptions par défaut.
* **Visibilité :** Changez `private: true` en `private: false` dans les `options` du nœud GitHub pour créer des dépôts publics par défaut.
* **Initialisation :** Changez `autoInit: true` en `false` si vous ne souhaitez pas que le dépôt soit initialisé avec un README.
* **Autres Options GitHub :** Explorez les autres options disponibles dans le nœud "GitHub" (operation: `create`, resource: `repository`) pour ajouter des .gitignore, des licences, etc.
* **Logique du Workflow :** Ajoutez d'autres nœuds après la création du dépôt pour effectuer des actions supplémentaires (ex: ajouter des collaborateurs, créer des branches, envoyer une notification Slack/Email).

## Contribution

Les contributions sont les bienvenues ! Si vous avez des améliorations à proposer ou des bugs à signaler :

1.  **Signaler un problème :** Ouvrez une "Issue" sur le dépôt GitHub (si applicable) pour discuter des changements que vous souhaitez apporter ou pour signaler un bug.
2.  **Proposer une modification (si via un dépôt Git) :**
    * Forkez le dépôt.
    * Créez une nouvelle branche (`git checkout -b feature/amelioration-x`).
    * Faites vos modifications.
    * Commitez vos changements (`git commit -am 'Ajout de telle fonctionnalité'`).
    * Poussez vers la branche (`git push origin feature/amelioration-x`).
    * Ouvrez une "Pull Request".

## Licence

Ce workflow n8n (le fichier JSON) est distribué sous la licence MIT. Voir le fichier `LICENSE` pour plus de détails (si fourni dans le dépôt), ou consultez [opensource.org/licenses/MIT](https://opensource.org/licenses/MIT).
