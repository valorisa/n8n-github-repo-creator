Comment Utiliser :

Copier le JSON : Copie l'intégralité du bloc de code JSON ci-dessus.

Créer un Fichier : Colle le code dans un éditeur de texte et enregistre-le sous un nom finissant par .json (par exemple, github_create_repo.json).

Importer dans n8n :

Ouvre ton interface n8n.

Va dans "Workflows".

Clique sur "Import" > "Import from File..." et sélectionne le fichier .json que tu viens de créer.

OU clique sur "Import" > "Import from URL/JSON" et colle directement le code JSON, puis clique sur "Import".

Configurer les Credentials :

Double-clique sur le nœud "Créer Dépôt GitHub".

Dans la section "Credentials", sélectionne tes identifiants d'accès GitHub préconfigurés dans n8n. Si tu n'en as pas, tu devras en créer (probablement de type "GitHub Access Token" ou "GitHub OAuth2"). Remplace "YOUR_GITHUB_CREDENTIALS_ID" par l'ID réel de tes credentials dans le JSON avant d'importer, ou sélectionne-les dans l'interface après l'import.

Modifier les Détails (Optionnel) :

Double-clique sur le nœud "Définir Détails Dépôt".

Modifie les valeurs pour repoName, description, visibility, initialize, etc., selon tes besoins avant d'exécuter le workflow.

Tester : Sauvegarde le workflow et clique sur "Execute Workflow" pour le tester.

Ce workflow te fournit une base solide pour automatiser la création de dépôts GitHub. Tu peux l'adapter en modifiant les options dans le nœud "Set" ou en ajoutant d'autres étapes (par exemple, ajouter des collaborateurs, créer des branches, etc.) en utilisant d'autres opérations du nœud GitHub.
