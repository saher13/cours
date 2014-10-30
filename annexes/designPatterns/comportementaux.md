# Chain of responsability (Chaine de responsabilité) 

Evite le couplage de l'émetteur d'une requête à ses récepteurs, en donnant à 
plus d'un objet la possibilité d'entreprendre la requête. Chaîne les objets 
récepteurs et fait passer la requête tout au long de la chaîne, jusqu'à ce qu'un 
objet la traite. 

## Quand 

- une requête peut être gérée par plus d'un objet à la fois, et que le gestionnaire 
n'est *a priori* pas connu. Ce dernier doit être déterminé automatiquement 
- on souhaite adresser une requête à un ou plusieurs objets, sans spécifier 
explicitement le récepteur
- l'ensemble des objets qui peuvent traiter une requête doit être défini 
dynamiquement. 

## Conséquences 

- réduction du couplage
- souplesse accrue dans l'attribution de responsabilités aux objets 
- la réponse n'est pas garantie 

# Commande 

Encapsule une requête comme un objet, autorisant ainsi le paramétrage des 
clients par différentes requêtes, files d'attente et récapitulatifs de requêtes, tout 
en permettant la réversion des opérations. 

## Quand 

- on veut introduire dans des objets des actions à effectuer (sous forme de 
paramètres)
- on veut spécifier, mettre en file d'attende, et exécuter les requêtes à différents 
instants. Une commande a une durée de vie indépendante de la requête originale
- on souhaite assurer le service de réversion (*undo*)
- on permet une mémorisation de modifications, afin de pouvoir les ré-appliquer à 
nouveau
- on veut structurer un système autour d'opérations de haut niveau

## Conséquences 

- supprime tout couplage entre l'objet qui invoque une opération, et celui qui sait 
comment la réaliser
- les commandes étant des objets, ils peuvent être manipulés comme tout autre 
objet
- on peut assembler des commandes dans une commande composite
- il est facile d'ajouter de nouveaux objets *Commande*, car on ne modifie pas 
les classes existantes 

# Interprète (Interpreter)

Pour un langage donné, définit une représentation de sa grammaire, en même 
temps qu'un interprète utilisant cette représentation pour interpréter les 
phrases du langage. 

## Quand 

A utiliser quand on doit interpréter un langage, et que ses déclarations peuvent 
être représentées comme un AST. L'interprète est optimal quand : 

- la grammaire est simple
- l'efficacité n'est pas un souci majeur

## Conséquences 

- il est facile de modifier et d'étendre la grammaire 
- le codage de la grammaire est facile
- les grammaires complexes sont difficiles à maintenir
- facilité d'addition de nouveaux modes d'interprétation des expressions

# Iterator (Itérateur)

Fournit un moyen d'accès séquentiel aux éléments d'un agrégat d'objets, sans 
mettre à découvert sa représentation interne. 

## Quand 

- on veut accéder au contenu d'un objet d'un agrégat sans briser l'encapsulation 
- on gère simultanément plusieurs parcours dans des agrégats d'objets 
- on souhaite offrir une interface uniforme pour les parcours aux travers de 
diverses structures d'aggrégat (itération polymorphe)

## Conséquences 

- on permet des modifications dans le parcours des agrégats
- l'interface des classes d'agrégats sont simplifiées 
- il peut y avoir plusieurs parcours en cours sur un même agrégat

# Mediator (Médiateur) 

Définit un objet qui encapsule les modalités d'interaction d'un certain ensemble 
d'objets. Le médiateur favorise le couplage faible en dispensant les objets de se 
faire explicitement référence, et il permet donc de faire varier indépendamment 
les relations d'interaction.

## Quand 

- les objets d'un ensemble communiquent de façon bien définie mais très 
complexe. Le résultat des interdépendances est non structuré et difficile à 
appréhender
- la réutilisation d'un objet est difficile, du fait qu'il fait référence à beaucoup 
d'autres objets et communique avec eux
- un comportement distribué entre plusieurs classes doit pouvoir être spécialisé 
sans une pléthore de dérivation

## Conséquences 

- limtate la création de sous-classes
- réduit le couplage entre collègues
- simplifie les protocoles objet
- formalise la coopération des objets
- centralise le contrôle

# Memento 

Sans violer l'encapsulation, saisit et transmet à l'extérieur d'un objet, l'état 
interne de celui-ci, afin de pouvoir ultérieurement le restaurer dans cet état. 

## Quand 

- une capture de tout ou d'une partie de l'état doit être sauvegardée, pour être 
restaurée plus tard
- l'utilisation d'une interface direct pour atteindre l'état conduirait à réveler des 
détails d'implémentation, donc à rompre l'encapsulation

## Conséquences 

- préservation des frontières d'encapsulation
- simplification de l'auteur
- utilisation potentiellement coûteuse
- définition d'interfaces étroites et larges, difficulté de garantir que seul l'auteur 
aura accès à l'état du Memento
- coûts cachés de la surveillance des Mementos

# Observer (Observateur)

Définit une interdépendance de type *un-plusieurs*, de sorte que quand un objet 
change d'état, tous ceux qui en dépendent en soient notifiés et mis à jour. 

## Quand 

- un concept a deux (ou plus) représentations dépendantes, afin de les modifier 
indépendamment
- la modification d'un objet nécessite la modification d'autres, sans savoir 
combien sont ces "autres"
- un objet doit pouvoir notifier d'autres objets, sans faire d'hypothèse sur leurs 
natures, quand ils ne doivent pas être couplés

## Conséquences 

- modification indépendante des sujets et des observateurs, couplage minimal
- support de la diffusion 
- mises à jour inopinées (cascade de mises à jours)

# State (Etat) 

Permet à un objet de modifier son comportement quand son état interne 
change : l'objet semble changer de classe. 

## Quand 

- le comportement d'un objet dépend de son état, et ce changement de 
comportement intervient dynamiquement selon cet état
- les opérations comportent de gros paquets de conditions sur l'état de l'objet. 

## Conséquences 

- isole les comportements spécifiques d'états et partitionne les différents 
comportements état par état
- rend les transitions d'état plus explicites
- les *Etats* peuvent être partagés 

# Strategy (Stratégie) 

Définit une famille d'algorithmes interchangeables et encapsulés. Permet aux 
algorithmes d'évoluer indépendamment des clients qui les utilisent. 

## Quand 

- plusieurs classes apparentées ne diffèrent que par leur comportement. 
- on a besoin de diverses variantes d'un algorithme 
- un algorithme utilise des données que les clients n'ont pas à connaître
- une classe définit de nombreux comportements selon des conditions multiples 

## Conséquences 

- on fait une famille d'algorithmes apparentés : on peut isoler les fonctionnalités 
communes
- solution alternative à la dérivation en sous-classes 
- dispense de déclarations conditionnelles 

# Template Method (Patron de méthode) 

Définit le squelette d'un algorithme en déléguant certaines étapes à des 
sous-classes. Certaines parties de l'algorithme peuvent donc être redéfinies sans 
avoir à modifier sa structure. 

## Quand 

- on implémente 1 fois les parties invariantes d'un algorithme, et on veut laisser 
aux sous-classes l'implémentation des parties conçues comme modifiables
- pour éviter la duplication de code, il faut isoler le facteur commun des 
comportements pour l'implanter dans une classe commune 
- on veut contrôler des extensions de sous-classes

## Conséquences 

- réutilisation du code 
- extension du comportement de la classe parente 
- oubli de l'opération héritée

# Visitor (Visiteur) 

Fait la représentation d'une opération applicable aux éléments d'une strcture 
d'objet. Permet de redéfinir une nouvelle opération, sans avoir à modifier la 
classe des éléments sur lesquels elle agit. 

## Quand 

- une structure d'objets contient beaucoup de classes différentes d'interface 
distinctes, et on veut effectuer des opérations sur ces objets, qui dépendent de 
leur classe concrète
- on doit effectuer plusieurs opérations distinctes et sans relation, sur les objets 
d'une structure, tout en évitant de polluer leurs classes avec ces opérations
- les classes qui définissent la structure changent rarement, mais qu'on a 
souvent besoin de définir de nouvelles opérations sur cette structure

## Conséquences 

- facilité d'addition de nouvelles opérations 
- rassemblement des opérations du même type 
- isolement des opérations non apparentées 
- addition de nouveaux élements difficiles 
- tournée à travers la hiérarchie des classes 
- garde des informations d'état
- casse l'encapsulation en obligeant à rendre publiques des opérations sur l'état 
interne