# Meilleures pratiques pour Azure DevOps et Git Flow

## 1. Gestion des branches (Git Flow)

### Structure des branches
- `main` : Représente la version en production.
- `develop` : Branche principale de développement.
- `feature/*` : Pour le développement de nouvelles fonctionnalités.
- `release/*` : Pour la préparation des versions de production.
- `hotfix/*` : Pour les correctifs urgents en production.

### Création et fusion des branches
1. Créez une branche `feature` à partir de `develop` :
   ```
   git checkout develop
   git checkout -b feature/nouvelle-fonctionnalite
   ```
2. Une fois la fonctionnalité terminée, fusionnez dans `develop` via une Pull Request.
3. Pour une release, créez une branche `release` à partir de `develop` :
   ```
   git checkout develop
   git checkout -b release/1.0.0
   ```
4. Après les tests, fusionnez `release` dans `main` et `develop`.

### Convention de nommage des commits
Utilisez des messages de commit clairs et descriptifs. Exemple de structure :
```
<type>(<portée>): <description>

[corps optionnel]

[pied de page optionnel]
```

Types courants : `feat`, `fix`, `docs`, `style`, `refactor`, `test`, `chore`.

Exemple :
```
feat(auth): ajouter l'authentification par OAuth

- Implémente le flux d'authentification OAuth
- Ajoute des tests pour le nouveau processus
- Met à jour la documentation utilisateur

Closes #123
```

Exemple de commande git commit complète :
```bash
git commit -m "feat(auth): ajouter l'authentification par OAuth

- Implémente le flux d'authentification OAuth
- Ajoute des tests pour le nouveau processus
- Met à jour la documentation utilisateur

Closes #123" --signoff
```

Explication de la commande :
- `-m` permet de spécifier le message de commit.
- Le message est entre guillemets et contient plusieurs lignes.
- La première ligne est le titre du commit, suivant le format `<type>(<portée>): <description>`.
- Deux lignes vides séparent le titre du corps du message.
- Le corps du message contient une liste à puces des changements principaux.
- Une ligne vide sépare le corps du pied de page.
- Le pied de page fait référence à un numéro d'issue avec "Closes #123".
- `--signoff` ajoute une ligne "Signed-off-by" avec votre nom et email, ce qui est une bonne pratique dans certains projets.

## 2. Bonnes pratiques pour les Pull Requests

1. Utilisez des titres descriptifs.
2. Fournissez une description détaillée des changements.
3. Liez la PR aux work items correspondants.
4. Demandez une revue à au moins deux développeurs.
5. Utilisez des labels pour catégoriser les PRs (ex: `bug`, `enhancement`).
6. Configurez des checks automatiques (tests, linting) avant la fusion.

## 3. Configuration du pipeline CI/CD

1. Utilisez des variables pour les valeurs sensibles ou spécifiques à l'environnement.
2. Divisez le pipeline en étapes logiques (build, test, deploy).
3. Utilisez des templates pour réutiliser la configuration entre projets.
4. Configurez des environnements différents (dev, staging, prod).

Exemple de structure de pipeline YAML :
```yaml
trigger:
  - main
  - develop

variables:
  - group: project-variables

stages:
  - stage: Build
    jobs:
      - job: BuildJob
        steps:
          - script: echo "Building the project"
          
  - stage: Test
    jobs:
      - job: TestJob
        steps:
          - script: echo "Running tests"
          
  - stage: Deploy
    jobs:
      - deployment: DeployWeb
        environment: production
        strategy:
          runOnce:
            deploy:
              steps:
                - script: echo "Deploying to production"
```

## 4. Utilisation efficace d'Azure Boards

1. Utilisez des User Stories pour décrire les fonctionnalités du point de vue de l'utilisateur.
2. Décomposez les User Stories en tâches plus petites.
3. Utilisez des étiquettes pour catégoriser les work items.
4. Liez les work items aux branches et PRs correspondantes.
5. Utilisez des requêtes pour suivre le progrès et identifier les blocages.

## 5. Sécurité et gestion des accès

1. Appliquez le principe du moindre privilège.
2. Utilisez Azure AD pour l'authentification.
3. Activez l'authentification à deux facteurs.
4. Révisez régulièrement les accès.
5. Utilisez des secrets sécurisés pour stocker les informations sensibles.


## 6. Monitoring et logging

1. Configurez des alertes sur les métriques clés du pipeline.
2. Utilisez Application Insights pour le monitoring en production.
3. Centralisez les logs pour une analyse facile.

## 9. Documentation

1. Maintenez un README.md à jour dans chaque repo.
2. Utilisez le wiki d'Azure DevOps pour la documentation du projet.
3. Documentez l'architecture et les décisions importantes.

## 10. Tests

1. Visez une couverture de tests élevée (>80%).
2. Incluez des tests unitaires, d'intégration et end-to-end.
3. Exécutez les tests automatiquement dans le pipeline CI/CD.
4. Utilisez des outils de qualité de code (ex: SonarQube).

## 11. Revues de code

1. Établissez des guidelines de revue de code.
2. Utilisez des pull request templates pour standardiser les informations fournies.
3. Encouragez les revues de code constructives et bienveillantes.

## 12. Bonnes pratiques pour les Pull Requests

1. Utilisez des titres descriptifs.
   Exemple : "Feat: Implémentation de l'authentification OAuth pour l'API utilisateur"

2. Fournissez une description détaillée des changements.
   Exemple :
   ```
   Cette PR implémente l'authentification OAuth pour notre API utilisateur.

   Changements principaux :
   - Ajout d'un middleware OAuth dans server.js
   - Création d'un nouveau contrôleur oauthController.js
   - Mise à jour des routes utilisateur pour utiliser l'authentification OAuth
   - Ajout de tests unitaires et d'intégration pour le nouveau flux d'authentification
   - Mise à jour de la documentation API dans le README.md

   Cette implémentation permettra aux utilisateurs de se connecter via leur compte Google ou GitHub.
   ```

3. Liez la PR aux work items correspondants.
   Dans la description de la PR, ajoutez : "Résout AB#123" où 123 est l'ID du work item dans Azure Boards.

4. Demandez une revue à au moins deux développeurs.
   Utilisez la fonctionnalité "Reviewers" d'Azure DevOps pour assigner des relecteurs.

5. Utilisez des labels pour catégoriser les PRs (ex: `bug`, `enhancement`, `documentation`).

6. Configurez des checks automatiques (tests, linting) avant la fusion.
   Dans les paramètres de la branche, configurez des règles de protection comme "Require a successful build" et "Require code review".

En suivant ces meilleures pratiques, vous optimiserez votre utilisation d'Azure DevOps et améliorerez l'efficacité et la qualité de votre processus de développement.

# Exemple de Gitflow : De la création du projet au déploiement

Ce tutoriel vous guidera à travers la mise en place d'un projet Python sur Azure DevOps en utilisant les meilleures pratiques de GitFlow.

## 1. Configuration initiale sur Azure DevOps

1. Connectez-vous sur [dev.azure.com](https://dev.azure.com).
2. Créez un nouveau projet : 
   - Cliquez sur "New project".
   - Nommez-le "MonProjetPython".
   - Choisissez la visibilité (privée recommandée pour la plupart des projets).
   - Cliquez sur "Create".

3. Initialisez le repo :
   - Allez dans "Repos" dans le menu de gauche.
   - Initialisez avec un README.

## 2. Configuration de GitFlow

1. Clonez le repo localement :
   ```
   git clone <URL_du_repo>
   cd MonProjetPython
   ```

2. Créez la branche `develop` :
   ```
   git checkout -b develop
   git push -u origin develop
   ```

3. Dans Azure DevOps, allez dans "Repos" > "Branches" et définissez `develop` comme branche par défaut.

## 3. Configuration de l'environnement Python

1. Créez un environnement virtuel :
   ```
   python -m venv venv
   source venv/bin/activate  # Sur Windows : venv\Scripts\activate
   ```

2. Créez un `requirements.txt` :
   ```
   pytest==6.2.5
   ```

3. Installez les dépendances :
   ```
   pip install -r requirements.txt
   ```

## 4. Développement d'une fonctionnalité

1. Créez une branche de fonctionnalité :
   ```
   git checkout -b feature/calcul-factoriel
   ```

2. Créez `main.py` avec le contenu suivant :

```python
def calculate_factorial(n: int) -> int:
    """
    Calculate the factorial of a non-negative integer.

    Args:
        n (int): The number to calculate the factorial of.

    Returns:
        int: The factorial of n.

    Raises:
        ValueError: If n is negative.
    """
    if n < 0:
        raise ValueError("Factorial is not defined for negative numbers.")
    if n == 0 or n == 1:
        return 1
    return n * calculate_factorial(n - 1)

if __name__ == "__main__":
    print(f"Factorial of 5 is: {calculate_factorial(5)}")
```

3. Créez `tests/test_main.py` :

```python
import pytest
from main import calculate_factorial

def test_calculate_factorial():
    assert calculate_factorial(0) == 1
    assert calculate_factorial(1) == 1
    assert calculate_factorial(5) == 120
    with pytest.raises(ValueError):
        calculate_factorial(-1)
```

4. Commitez vos changements :
   ```
   git add .
   git commit -m "feat(math): ajouter la fonction de calcul du factoriel

   - Implémente la fonction calculate_factorial
   - Ajoute des tests unitaires pour la fonction
   
   Closes #1" --signoff
   ```

5. Poussez la branche et créez une Pull Request :
   ```
   git push -u origin feature/calcul-factoriel
   ```
   Allez sur Azure DevOps, dans "Repos" > "Pull requests", et créez une nouvelle PR de `feature/calcul-factoriel` vers `develop`.

## 5. Revue de code et fusion

1. Dans la PR, ajoutez une description détaillée des changements.
2. Assignez des relecteurs.
3. Une fois la revue terminée et les tests passés, fusionnez la PR dans `develop`.

## 6. Préparation d'une release

1. Créez une branche de release :
   ```
   git checkout develop
   git pull
   git checkout -b release/1.0.0
   ```

2. Mettez à jour la version dans vos fichiers (par exemple, dans `setup.py` si vous en avez un).

3. Commitez les changements :
   ```
   git commit -m "chore(release): préparer la version 1.0.0" --signoff
   ```

4. Poussez la branche et créez une PR de `release/1.0.0` vers `main`.

## 7. Configuration du pipeline CI/CD

1. Dans Azure DevOps, allez dans "Pipelines" > "New pipeline".
2. Choisissez Azure Repos Git et sélectionnez votre repo.
3. Choisissez "Starter pipeline" et remplacez le contenu par :

```yaml
trigger:
  - main
  - develop

pool:
  vmImage: 'ubuntu-latest'

stages:
- stage: Build
  jobs:
  - job: BuildJob
    steps:
    - task: UsePythonVersion@0
      inputs:
        versionSpec: '3.8'
    - script: |
        python -m venv venv
        source venv/bin/activate
        pip install -r requirements.txt
      displayName: 'Install dependencies'
    - script: |
        source venv/bin/activate
        pytest tests/
      displayName: 'Run tests'

- stage: Deploy
  condition: and(succeeded(), eq(variables['Build.SourceBranch'], 'refs/heads/main'))
  jobs:
  - deployment: DeployProd
    environment: production
    strategy:
      runOnce:
        deploy:
          steps:
          - script: echo "Déploiement en production"
```

4. Sauvegardez et exécutez le pipeline.

## 8. Finalisation de la release

1. Une fois la PR de release approuvée et les tests passés, fusionnez-la dans `main`.
2. Créez un tag pour la release :
   ```
   git checkout main
   git pull
   git tag -a v1.0.0 -m "Version 1.0.0"
   git push origin v1.0.0
   ```

3. Fusionnez `main` dans `develop` :
   ```
   git checkout develop
   git merge main
   git push origin develop
   ```

## 9. Gestion du projet avec Azure Boards

1. Dans Azure DevOps, allez dans "Boards" > "Work items".
2. Créez de nouvelles User Stories pour les prochaines fonctionnalités.
3. Décomposez ces stories en tâches plus petites.
4. Assignez les tâches aux membres de l'équipe.

Ce tutoriel couvre les bases de l'utilisation d'Azure DevOps avec GitFlow, de la création du projet jusqu'au déploiement. Continuez à itérer sur ce processus pour les futures fonctionnalités et releases.
