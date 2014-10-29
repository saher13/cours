# Adaptateur (Adapter) 

Convertit l'interface d'une classe en une autre, conforme à l'attente du client. 
L'adaptateur permet à des classes de collaborer, qui n'auraient pas pu le faire à 
cause d'interfaces incompatibles. 

## Quand

- on veut utiliser une classe existante, mais dont l'interface ne coïncide pas avec 
celle escomptée
- on souhaite créer une classe réutilisable qui collabore avec des classes sans 
relations avec elle et encore inconnues, càd avec des classes qui n'auront pas 
nécessairement des interfaces compatibles
- on a besoin d'utiliser plusieurs sous-classes existantes, mais l'adaptation de leur 
interface par dérivation de chacune d'entre elles est impraticable. Un adapteur 
objet peut adapter l'interface de sa classe parente.

## Conséquences

Un adaptateur de classe : 

- adapte en s'en remettant à une classe ```Adaptateur``` concrète. Il en résulte 
qu'un adaptateur de classe ne fonctionnera pas si on essaie d'adapter une classe 
*et* toutes ses sous-classes
- permet à un adaptateur de redéfinir certaines des comportements de l'adapté, 
du fait que ```Adaptateur``` est une sous-classe de ```Adapté```
- introduit un seul objet, et aucun pointeur additionnel n'est requis pour atteindre 
l'```Adapté```. 

Un adapateur d'objet : 

- permet à un simple ```Adaptateur``` de travailler avec plusieurs adaptés, et 
toutes les sous-classes éventuelles. L'Adaptateur peut aussi ajouter des 
fonctionnalités à tous les adaptés en une seule fois
- rend plus difficile la surcharge du comportement de l'adapté. Il faut en fait la 
dériver, et impose que l'```Adaptateur``` fasse référence à la sous-classe 

# Bridge (Pont) 

Découple une abstraction de son implémentation afin que les deux éléments 
puissent être modifiés indépendamment l'un de l'autre. 

## Quand

- on souhaite éviter un lien définitif entre une abstraction et son implémentation. 
Ce peut être le cas lorsque l'implémentation doit être choisie parmi plusieurs ou 
échangée avec une autre lors de l'exécution
- il faut que les abstractions et leurs implémentations puissent toutes deux être 
étendues par dérivation. En ce cas, le modèle *Bridge* permet de combiner 
les différentes abstractions et implémentations, et de les étendre 
indépendamment. 
- il ne faut pas que les modifications apportées à l'implémentation d'une 
abstraction aient un impact sur le code client, qui ne devrait pas être recompilé 
- on souhaite cacher complètement l'implémentation d'une abstraction (C++)
- on est confronté à une prolifération de classes
- on veut faire partager une même implémentation à plusieurs objets, et cela 
doit être caché au client

## Conséquences

- découplage de l'interface et de l'implémentation
- capacité d'extension accrue
- dissimulation des détails d'une implémentation aux clients 

# Composite 

Compose des objets en une strcture arborescente pour représenter des 
hiérarchies composants/composé. Permet au client de traiter de la même façon 
(unique) les objets individuels et des combinaisons de ceux-ci. 

## Quand

- on veut représenter des hiérarchies de l'individu à l'ensemble 
- on souhaite que le client n'ait pas à se préoccuper de la différence entre 
combinaisons d'objets et objets individuels. Les clients pourront traiter de façon 
uniforme tous les objets de la strcture composite.

## Conséquences

- définit des hiérarchies de classes consistant en des objets primitifs et des objets 
composites. Les objets primitifs peuvent être combinés en des objets plus 
complexes, qui à leur tour peuvent être composés, etc... Partout où le client 
sollicite un objet primitif, on peut utiliser un objet composite
- simplifie le niveau client, qui peut traiter à l'identique les structures individuelles 
et composites. D'ailleurs, ils n'ont pas à savoir si l'objet est composite ou non
- rend plus facile l'adjonction de nouveaux types de composants, sans modifier le 
client 
- peut rendre une conception excessivement générale, car imposer des 
contraintes est plus difficile (sauf en contrôlant à l'exécution)

# Decorator (Décorateur)

Attache dynamiquement des responsabilités supplémentaires à un objet. Les 
décorateurs fournissent une alternative souple à la dérivation, pour étendre les 
fonctionnalités. 

## Quand

- pour ajouter dynamiquement des responsabilités à des objets individuels, de 
façon transparent, sans affecter les autres obets du même type
- pour des responsabilités qui doivent pouvoir être retirées
- quand l'extension par dérivation est impraticable (explosion des sous-classes 
d'extension)

## Conséquences

- offre plus de souplesse que l'héritage statique
- évite de surcharger de fonctionnalités les classes situées en haut de la 
hiérarchie
- un décorateur et ses composants ne sont pas identiques : un composant décoré 
n'est pas identique au composant lui-même
- il y a une multitude de petits objets

# Facade (Façade) 

Fournit une interface unifiée, à l'ensemble des interfaces d'un sous-système. La 
façade fournit une interface de plus haut niveau, qui rend le sous-système plus 
facile à utiliser. 

## Quand

- on souhaite disposer d'une interface simple pour un sous-système complexe. 
- il y a beaucoup de relations de dépendances entre les clients et les classes 
d'implémentation d'une abstraction
- on cherche à structurer en niveaux un sous-système

## Conséquences

- masque au client des composants du sous-système, donc réduit le nombre 
d'objets dont il faut tenir compte, rendant ainsi le système plus simple à utiliser
- favorise le couplage faible entre le sous-système et ses clients
- n'empêche pas les applications d'utiliser les classes du sous-système si 
nécessaire (pour exhaustivité).

# Flyweight (Poids mouche) 

Utilise une technique de partage qui permet la mise en oeuvre efficace d'un 
grand nombre d'objets de fine granularité. 

## Quand

Quand toutes ces conditions sont réunies : 

- l'application utilise un grand nombre d'objets
- les coûts de stockage sont élevés du fait d'une réelle quantité d'objets
- la plupart des états de l'objet peuvent être considérés comme extrinsèques
- plusieurs groupes d'objets peuvent être remplacés par un nombre relativement 
faible d'objets partagés, après que les états extrinsèques ont été retirés
- l'application ne dépend pas de l'identité des objets (les objets poids-mouche 
pouvant être partagés, et passer pour identiques) 

## Conséquences

Une économie de mémoire en fonction de : 

- la diminution du nombre total d'instances (qui résulte du partage)
- le nombre total d'états intrinsèques par objet
- le fait que l'état extrinsèque soit stocké ou calculé 

# Proxy (Procuration)

Fournit à un tiers objet un mandataire ou un remplaçant, pour contrôler l'accès à 
cet objet. 

## Quand

- une procuration *à distance* fournit un représentant local d'un objet situé dans 
un espace d'adresse différent
- une procuration *virtuelle* crée des objets lourds à la demande
- une procuration *de protection* contrôle l'accès à l'objet original (droits d'accès)
- une *référence intelligente* remplace un pointeur brut

## Conséquences

- une procuration distante cache le fait qu'un objet réside ailleurs 
- une procuration virtuelle peut effectuer des optimisations 
- une procuration de protection permettent un travail lors de l'accès à l'objet 