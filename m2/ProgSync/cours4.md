# Programmation synchrone - Cours 4 : Structures, tableaux, itérations

## Structures de données

Une variable est un flot de données. A chaque tick, il prend une valeur.  
On veut définir un nouveau type de flots. On prendra comme exemple les
variables représentant des nombres complexes.  

Pour définir un type, on peut ajouter un type au projet Scade. *On l'appelle
ici "Complexe", une structure qui a deux champs :*  
```Complexe = { re:real, im:real}```.

### Opérateurs prédéfinis pour les structures

Constructeur (Make) :  
![schéma 1](4_01.png)

Décomposition (Flatten) :  
![schéma 2](4_02.png)

Accesseur lecture statique :  
![schéma 3](4_03.png)

Affectation :  
![schéma 4](4_04.png)

#### Opération personnalisée

Produit :  
![schéma 5](4_05.png)

## Tableaux

Les **tableaux Scade** ont une taille fixe. Leur indice commence à 0.
Ils sont génériques, on peut faire des tableaux avec n'importe quel type.  
A chaque tick, chacun de ses éléments va avoir une valeur.  
Pour accèder aux éléments d'un tableau : ```t[3][7]```.  
On peut écrire des tableaux constants : ```[4, 8, 15, 16]```.  
On peut définir un type de tableaux : ```tab_R5 = real^5```.  

### Opérateurs

Constructeurs, decomposition, accesseur, affectation, concaténation.  

Dynamique :  
![schéma 6](4_06.png)

Reverse :
![schéma 7](4_07.png)

Slice :
![schéma 8](4_08.png)

## Itérateurs

![schéma 9](4_09.png)
