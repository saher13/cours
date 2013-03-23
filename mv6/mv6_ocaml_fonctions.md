# Fonctions de première classe

## La programmation fonctionnelle
Les fonctions sont des valeurs comme les autres. Des valeurs de premiere classe.
Une fonction de première classe aura comme signature : `fun x -> E`

### Valeur de première classe
Une valeur de première classe peut être :
* anonyme
* affecté a un identificateur
* stockée dans une structure de données (dans une liste par exemple)
* passée comme argument à une fonction
* retournée par une fonction

### Conséquences
* Les fonctions sont anonymes
* Les fonctions peuvent intervenir n'importe ou dans le code
* Les fonctions n'ont qu'un argument

## Fonctions n-aire
Les fonctions de première classe n'ont qu'un argument, mais il parfois nécessaire d'utiliser plusieurs arguments.
* regroupement des arguments en un seul n-uplet
* curryfication : la fonction renvoie en fait une fonction. Cela utilise/permet en fait l'application partielle.

### Remarque
OCaml évalue l'argument d'une fonction avant de lui passer. Cela implique l'évaluation __de droite à gauche__.

## Représentation mémoire des fonctions

## Sauvegarde et restauration lors des appels/retours

La représentation mémoire d'une fonction est appelée `Closure` (fermeture) : un bloc de tag 247 contenant, entre autres, l'offset (décalage) d'un saut vers le code de la fonction.

Nous représenterons cette offset par un label (marquant une position absolue dans le code) bien que celui-ci soit en fait relatif à l'endroit ou on se trouve.

### Appel de fonction
Il faut stocker :
* L'endroit où continuer l'execution à la fin de l'appel
  * On sauve `PC` sur la pile (sous l'argument de la fonction, qui est la première case de la pile) à l'appel
  * On restaure `PC` lors du retour
* Un environnement : une série de valeurs (définies en dehors du code de la fonction) que la fonction peut utiliser.
  * Closure enregistre une série de labels (offset des fonctions) + les élements de l'environnement
  * `env` pointe toujours sur Closure de la fonction courante

Donc, Closure stocke aussi les valeurs des l'environnement

To be continued...