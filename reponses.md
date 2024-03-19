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

**Y a-t-il des Security Hotspots ? Si oui, combien et pour quelle(s) raison(s) ?**

Oui, il y'en a 1 dans le DockerFile:

`FROM python:3.9`

- The python image runs with root as the default user. Make sure it is safe here.