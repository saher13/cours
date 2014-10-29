# Abstract Factory (Fabrique Absraite) 

Fournit une interface pour la création de familles d'objets apparentés ou 
interdépendants, sans qu'il soit nécessaire de spécifier leurs classes concrètes.

## Quand 

- un système doit être indépendant de la façon dont ses produits ont été créés, 
combinés, et représentés.
- un système doit être constitué à partir d'une famille de produits, parmi plusieurs
- on souhaire renforcer le caractère de communauté d'une famille d'objets 
produits cinçus pour être utilisés ensemble
- on souhaite fabriquer une bibliothèque de classes de produits, en n'en révélant 
que l'interface et non l'implémentation

## Conséquences 

- isole les classes concrètes 
- facilite la substitution de familles de produits 
- favorise le maintien de la cohérence entre les objets, si ils sont conçus pour 
travailler avec une même famille
- il est difficile de gérer de nouveaux types de produits 

# Builder (Monteur) 

Dissocie la construction d'un objet complexe de sa représentation, de sorte que 
le même processus de construction permette des représentations différentes. 

## Quand 

- l'algorithme de création d'un objet complexe doit être indépendant des parties 
qui composent l'objet et de la manière dont ces parties sont agencées
- le processus de construction doit autoriser des représentations différentes de 
l'objet en construction

## Conséquences 

- permet de modifier la représentation interne d'un produit
- isole le code de construction et de représentation
- permet un meilleur contrôle du processus de construction 

# Factory (Fabrique)

Définit une interface pour la création d'un objet, mais en laissant à des 
sous-classes le choix des classes à instancier. La fabrication permet à une classe 
de déléguer l'instanciation à des sous-classes. 

## Quand 

- une classe ne peut prévoir la classe des objets qu'elle aura à créer
- une classe attend de ses sous-classes qu'elles spécifient les objets qu'elles créent
- les classes délèguent des responsabilités à une de leurs nombreuses 
sous-classes assistantes, et l'on veut disposer localement de l'information 
permettant de connaître la sous-classe assistante qui a reçu cette délégation.

## Conséquences 

- le client peut avoir à dériver
- produit un gîte aux sous-classes
- interconnecte des hiérarchies parallèles de classes

# Prototype 

Spécifie le type des objets à créer à partir d'une instance de prototype, et crée 
de nouveaux objets en copiant ce prototype. 

## Quand 

- si un système doit être indépendant de la manière dont ses produits sont créés, 
composés et représentés 
- si les classes à instancier sont spécifiées à l'exécution
- ou pour éviter de construire une hiérarchie de classes factory, qui réplique la 
hiérarchie de classes de produits
- ou encore si les instances d'une classe peuvent prendre un état parmi un petit 
nombre de combinaisons. Il est plus approprié d'installer le bon nombre de 
prototypes et de les cloner

## Conséquences 

- cache aux clients les classes de roduits concrets, réduisant le nombre de noms 
à connaître
- permet l'addition et la suppression de produits à l'exécution 
- permet la spécification de nouveaux objets par valeurs modifiables 
- spécification de nouveaux objets par structures modifiables 
- limitation du nombres de dérivations de classes 
- configuration dynamique d'une application avec des classes 

# Singleton 
