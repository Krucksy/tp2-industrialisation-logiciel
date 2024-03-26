# Exercice 1

## b)

**Quelles étapes (steps) sont réalisées par cette action ?**

1. Installation de python 3.10
2. Installation de pip, flake8 et les requirements.txt
3. Utilisation de flake8 pour vérifier la qualité du code
4. Lance les tests pytest

**Une étape est définie au minimum par 2 éléments, lesquels sont-ils et à quoi servent-ils ?**

- Le nom de l'étape
- Une commande à exécuter soit par *run*, soit par *uses*

**La première étape contient le mot-clé 'with', a quoi sert-il ?**

C'est utilisé pour définir les variables d'environnement.


# Exercice 2

## a)

**Sur l'onglet `Summary` d'une analyse de code, SonarCloud fournit 4 indicateurs. Quels sont-ils et quelles sont leurs utilités ?**

Bugs: Nombre de bugs dans le code
Vulnerabilities: Nombre de failles de sécurité dans le code (doivent être corrigées)
Code Smells: Nombre de mauvaises pratiques dans le code
Security Hotspots: Nombre de failles de sécurité potentielles dans le code (potentielles failles, donc pas obligatoirement à corriger)

**À quoi sert l'indicateur Quality Gate ?**

C'est un indicateur (booléen) qui démontre l'état du projet, si il est prêt à être déployé ou non.


## b)

**Quelle est la différence entre les sections `New code` et `Overall Code` dans l'onglet Summary ?**

`New code` calcule les indicateur uniquement sur le nouveau code qui a été ajouté. Alors que `Overall Code` calcule les indicateurs sur le code en entier, depuis le début du projet.

**Y a-t-il des Code Smells ? Si oui, combien et pour quelle(s) raisons(s) ?**

Oui, il y'en a 3.
- Remove the unused function parameter "deferred".
- Update this function so that its implementation is not identical to spend_cash on line 20.
- Remove the unused function parameter "deferred".

Pour corriger ce problème, il faut supprimer les paramètres inutiles.

**Y a-t-il des Security Hotspots ? Si oui, combien et pour quelle(s) raison(s) ?**

Oui, il y'en a 1 dans le DockerFile:

`FROM python:3.9`

- The python image runs with root as the default user. Make sure it is safe here.

Pour corriger ce problème, il faut créer un utilisateur non-root et l'utiliser pour lancer l'application.
Attention de le créer après le `pip install` pour ne pas avoir de problèmes de permissions.

```Dockerfile
RUN adduser --system --no-create-home nonroot

USER nonroot
```


# Exercice 3

## a)

**Que fait le job pytest ?**
Il récupère une image python, il y installe virtualenv, créer un nouvel environnement virtuel, installe les dépendances (requirements.txt) et lance les tests.

**Que fait le job image-creation ?**
Il va créer une image kaniko, une alternative à Docker.

**Que fait le job package-creation ?**
Il va créer un package python à partir du code source.

**Dans quel ordre les différents jobs s’executent-ils et pourquoi ?**
D'abord pytest, ensuite image-creation et enfin package-creation.
Car c'est l'ordre des 'stages' dans le fichier yaml.

**Le stage 2 génère une image Docker. Où est-elle stockée et comment pouvez-vous la retrouver ?**
Elle est disponible dans l'onglet `Container Registry` du projet.

**Le stage 3 génère un wheel Python. Où est-il stocké et comment pouvez-vous le retrouver ?**
Elle est publié sur la forge GitLab. Elle est disponible dans l'onglet `Package Registry` du projet.
Il est stocké dans un artifact du job package-creation. Précisément dans le dossier /dist.

## b)

Commande pour installer le package TP2 depuis le dépôt GitLab:

```bash
pip install TP2 --index-url https://__token__:<personal_token>@gitlab-etu.ing.he-arc.ch/api/v4/projects/3537/packages/pypi/simple
```

Attention, pour créer le token, il faut aller dans `Settings` -> `Access Tokens` -> `Create personal access token` et cocher `read_registry`. Et le role maintainer.

## c)

**A quel moment de la pipeline ce job s’execute-t-il et pourquoi ?**
A la fin, car il fait partie du stage `test-package` qui est en dernier dans la liste. 

**Que fait le job wheel-testing ?**
A partir de l'image python, il créer un environnement virtuel, installe le package TP2 et test si ce dernier fonctionne correctement.