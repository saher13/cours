# La machine virtuelle Ocaml

## Compilation
* ocamlopt : compile vers que code natif  
(IA-32, IA-64, AMD64, ARM, ...)
* ocamlc : compile vers du bytecode (cote-octet)  
(executé par ocamlrun)
* OCaml-Java (bytecode java), Ocamll (bytecode CIL), ...


## Le format de bytecode .cmo
http://cadmium.x9c.fr/distrib/caml-formats.pdf

# La machine virtuelle ocamlrun
Elle est écrite en C et boucle sur le cycle `fetch-decode-execute`
Voir le fichier `interp.c`  
Un execuble produit avec `ocamlc` commencera par `#!/usr/bin/ocaml`

## Etat de la machine constitué par :
* un programme : liste de bytecode
* un registre `PC` : adresse de la prochaine instruction à executer
* un registre `A` : l'accumulateur
* une pile `S` : 
* un tas `H` : espace pour placer les données allouées
* un environnement global `E`
* un registre `extra_args`
Rappel : registre = un espace pouvant contenir un mot machine

## Jeu d'instruction
Découpé en trois jeux :
* manipulation des valeurs immédiates : `int`, `char`, `bool`, `unit`
* représentation/manipulation des données structurées : `'a * 'b;`, `list`, `option`, `float`, `type ...`
* manipulation des fonctions de première classe : `'a -> 'b`

## Représentation des valeurs en mémoire
OCaml représente __toute__ valeur par un mot machine. i.e. taille fixe de 32/64 bits. Le premier bit défini son utilisation :
* 1 : valeur immédiate. La valeur est donc codée sur 31/63 bits
* 0 : pointeur vers un bloc

## Représentation des valeurs immédiates
Les valeurs immédiates sont donc codées sur un mot mémoire - 1 bit
* `int` : entier signé
* `char` : entier correspondant au code	ASCII
* `bool` : false = 0, true = 1
* `unit` : () = 0

### Gestion de la pile


## Représentation des autre valeurs (les blocs)

Ces valeurs sont stockées sur le tas. Les blocs sont allouées par la MV par malloc.


### Rappel sur le tas
Lors de la création d'un nouveau processus, l'OS lui assigne :

* un espace mémoire constant pour stocker le code
* un espace pour ses données constantes
* un espace de `pile` extensible (mais monotone) !!! PRECISION !!!
* de l'espace à la demande de l'utilisateur (malloc & free)

La zone mémoire pointée par le retour de malloc fait partie du `tas`. Le processus est chargé de libérer cette mémoire après utilisation.

### Un bloc : zone contigue dans la mémoire de n + 1 mots.
Constitué de deux "parties" :
* __en-tête__ (1 mot) :
  * `tag` (8 bits) : identifie le type de bloc
  * `color` (2 bits) : !!! A COMPLETER !!!
  * `wosize` (22 bits) : taille n de la zone de données

```
Extrait de mlvalues.h

For 16-bit and 32-bit architectures:
     +--------+-------+-----+
     | wosize | color | tag |
     +--------+-------+-----+
bits  31    10 9     8 7   0
```

* __données__ (n mots)

Pour connaitre les valeurs des tags : `mlvalue.h`, dont voici un extrait :

### Les constructeurs

* Les constructeur constant (sans mot clé __of__) sont représentées par des entiers constants. Chaque constructeur est numéroté à partir de 0.  
Un type ne peut donc pas posseder plus de 31^2 (ou 63^2) constructeurs constants. !!! A VERIFIER !!!
* Les constructeur paramétrés sont représentés par des blocs.

```
type t = A | B of int | C | D of t;;
```

* A : valeur immédiate 0
* C : valeur immédiate 1

```
      +-+-----+       +-+-----+
  A:  |1|  0  |   C:  |1|  1  |
      +-+-----+       +-+-----+
bit    0 1  31         0 1  31
```

* B 52 : bloc de taille 1 (2 mots)

```
         +-------------------------------+
         |+-+-----+---+-----+  +-+------+|
  B 42:  ||0|  1  | 0 |  1  |  |1|  52  ||
         |+-+-----+---+-----+  +-+------+|
         +-------------------------------+
bit        0 1   7 8 9 10 31    0 1   31
```

* D (D C) : deux blocs de taille 1

```
               +-------------------------------+     +-------------------------------+
               |+-+-----+---+-----+  +-+------+|     |+-+-----+---+-----+  +-+------+|
  D (D C) : b1 ||0|  1  | 0 |  1  |  |0|  b2  ||  b2 ||0|  1  | 0 |  1  |  |1|  1   ||
               |+-+-----+---+-----+  +-+------+|     |+-+-----+---+-----+  +-+------+|
               +-------------------------------+     +-------------------------------+
bit              0 1   7 8 9 10 31    0 1   31         0 1   7 8 9 10 31    0 1   31
```
