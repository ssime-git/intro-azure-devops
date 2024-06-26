# Tutoriel complet : Utilisation d'Azure DevOps avec Python

Ce tutoriel vous guidera à travers les étapes de mise en place d'un projet Python sur Azure DevOps, de la création du projet à la configuration d'un pipeline CI/CD.

## 1. Connexion et création d'un projet sur Azure DevOps

1. Rendez-vous sur [dev.azure.com](https://dev.azure.com) et connectez-vous avec votre compte Microsoft.
2. Cliquez sur "New project" en haut à droite.
3. Nommez votre projet (par exemple, "MonProjetPython").
4. Choisissez la visibilité (publique ou privée).
5. Cliquez sur "Create".

## 2. Création d'un nouveau repository

1. Dans votre nouveau projet, allez dans "Repos" dans le menu de gauche.
2. Cliquez sur le nom du repo actuel en haut, puis sur "New repository".
3. Nommez votre repo (par exemple, "mon-app-python").
4. Choisissez la visibilité.
5. Cochez "Initialize with a README".
6. Cliquez sur "Create".

## 3. Cloner le repository en local

1. Dans votre repo, cliquez sur "Clone" en haut à droite.
2. Copiez l'URL HTTPS.
3. Ouvrez un terminal sur votre machine locale.
4. Exécutez : `git clone <URL_copiée>`

## 4. Configuration de l'environnement Python

1. Naviguez vers le dossier du repo cloné : `cd mon-app-python`
2. Créez un environnement virtuel :
   ```
   python -m venv venv
   source venv/bin/activate  # Sur Windows, utilisez `venv\Scripts\activate`
   ```
3. Créez un fichier `requirements.txt` avec le contenu suivant :
   ```
   pytest==6.2.5
   ```
4. Installez les dépendances : `pip install -r requirements.txt`

## 5. Création du code Python

1. Créez un fichier `main.py` à la racine du projet avec le contenu suivant :

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


def is_prime(n: int) -> bool:
    """
    Check if a number is prime.

    Args:
        n (int): The number to check.

    Returns:
        bool: True if the number is prime, False otherwise.
    """
    if n < 2:
        return False
    for i in range(2, int(n**0.5) + 1):
        if n % i == 0:
            return False
    return True


if __name__ == "__main__":
    print(f"Factorial of 5 is: {calculate_factorial(5)}")
    print(f"Is 17 prime? {is_prime(17)}")
```

## 6. Ajout des tests

1. Créez un dossier `tests` à la racine du projet.
2. Dans le dossier `tests`, créez un fichier `test_main.py` avec le contenu suivant :

```python
import pytest
from main import calculate_factorial, is_prime


def test_calculate_factorial():
    assert calculate_factorial(0) == 1
    assert calculate_factorial(1) == 1
    assert calculate_factorial(5) == 120
    with pytest.raises(ValueError):
        calculate_factorial(-1)


def test_is_prime():
    assert is_prime(2) == True
    assert is_prime(17) == True
    assert is_prime(4) == False
    assert is_prime(1) == False
```

## 7. Commit et push des changements

Dans le terminal, exécutez :
```sh
git add .
git commit -m "Ajout des fonctions Python et des tests"
git push origin main
```

## 8. Création d'une branche pour une nouvelle fonctionnalité

1. Dans Azure DevOps, allez dans "Repos" > "Branches".
2. Cliquez sur "New branch".
3. Nommez votre branche (ex: "feature/nouvelle-fonction").
4. Choisissez "main" comme branche source.
5. Cliquez sur "Create".

## 9. Modification du code et création d'une Pull Request

1. Basculez sur la nouvelle branche localement : `git checkout feature/nouvelle-fonction`
2. Ajoutez une nouvelle fonction dans `main.py`, par exemple :

```python
def fibonacci(n: int) -> int:
    """
    Calculate the nth Fibonacci number.

    Args:
        n (int): The position in the Fibonacci sequence.

    Returns:
        int: The nth Fibonacci number.
    """
    if n <= 1:
        return n
    return fibonacci(n-1) + fibonacci(n-2)
```

3. Ajoutez un test pour cette fonction dans `test_main.py` :

```python
def test_fibonacci():
    assert fibonacci(0) == 0
    assert fibonacci(1) == 1
    assert fibonacci(5) == 5
    assert fibonacci(10) == 55
```

4. Commitez et poussez les changements :
   ```sh
   git add .
   git commit -m "Ajout de la fonction fibonacci"
   git push origin feature/nouvelle-fonction
   ```

5. Dans Azure DevOps, allez dans "Repos" > "Pull requests".
6. Cliquez sur "New pull request".
7. Choisissez votre branche comme source et "main" comme destination.
8. Ajoutez un titre et une description.
9. Cliquez sur "Create".

## 10. Configuration d'un pipeline CI/CD

1. Dans Azure DevOps, allez dans "Pipelines" dans le menu de gauche.
2. Cliquez sur "New pipeline".
3. Choisissez "Azure Repos Git" comme emplacement de votre code.
4. Sélectionnez votre repo.
5. Choisissez "Starter pipeline".
6. Remplacez le contenu du fichier YAML par :

```yaml
trigger:
- main

pool:
  vmImage: 'ubuntu-latest'

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
    python -m pytest tests/
  displayName: 'Run tests'
```

7. Cliquez sur "Save and run".

## 11. Utilisation d'Azure Boards pour la gestion de projet

1. Allez dans "Boards" dans le menu de gauche.
2. Cliquez sur "New Work Item" pour créer une tâche.
3. Remplissez les détails de la tâche (ex: "Implémenter la fonction de tri").
4. Assignez la tâche à un membre de l'équipe.
5. Définissez un état (To Do, Doing, Done).

## 12. Configuration des notifications

1. Cliquez sur l'icône utilisateur en haut à droite.
2. Sélectionnez "Notifications".
3. Cliquez sur "New subscription".
4. Choisissez le type d'événement (ex: "A build completes").
5. Configurez les détails de la notification.
6. Cliquez sur "Finish".

Ce tutoriel couvre les bases pour démarrer un projet Python avec Azure DevOps, incluant la gestion de code, les tests, l'intégration continue, et la gestion de projet. N'hésitez pas à explorer davantage chaque section pour découvrir toutes les fonctionnalités offertes par la plateforme.
