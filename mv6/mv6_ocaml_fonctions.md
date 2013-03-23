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

Nous représenterons cet offset par un label (marquant une position absolue dans le code) bien que celui-ci soit en fait relatif à l'endroit ou on se trouve.

### Appel de fonction
Il faut stocker :
* L'endroit où continuer l'execution à la fin de l'appel
  * Lors de l'appel : on sauve `PC`, `env` et `extra_args` __sur la pile__ (sous l'argument de la fonction, qui est la première case de la pile).
  * Lors du retour : On dépile et restaure `PC`, `env` et `extra_args`.
* Un environnement : une série de valeurs (définies en dehors du code de la fonction) que la fonction peut utiliser.
  * Closure enregistre :
     * une série de labels (i. e. offset des fonctions)
     * les élements de l'environnement
  * `env` pointe toujours sur Closure de la fonction courante

## Optimisation des fonctions n-aires

!!! A COMPLETER !!!


## Optimisation des appels terminaux (tail calls)

### Problème
Souvent, l'appel à une fonction est la derniere instruction avant de rendre la main.

```
let f x = x + 1
let g x = f x
```

Supposons que nous nous trouvions a executer g 0. On va :

1. Sauvegarder  `PC1`, `env1` et `extra_args1` avant l'appel à g.
2. Sauvegarder `PC2`, `env2` et `extra_args2` avant l'appel à f.
3. Mettre le resultat de f dans `A`
4. Restaurer `PC2`, `env2` et `extra_args2`
5. Restaurer `PC1`, `env1` et `extra_args1`

On perd du temps et de l'espace à sauvegarder/restaurer des informations qui ne nous servent pas. `PC2`, `env2` et `extra_args2` sauvegardés pour ensuite etre restaurés alors qu'ils seront de suite éffacés par la restauration de `PC1`, `env1` et `extra_args1`.

### Optimisation de ocamlrun
Les appels terminaux vont sauter les étapes 2 et 4. Celà est très important pour les fonctions recursives. Une fonction récursive ou dont tous les appels sont terminaux s'effectuera en pile constante (comme une boucle). On peut même du coup parler de fonction itérative. C'est un détails important qui peut éviter le problème du _stackoverflow_.
