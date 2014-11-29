# Programmation synchrone - Cours 3 : Automates

## Automates

Un noeud SCADE contient les équations, les diagrammes de flots, mais aussi les
automates. Ceux-ci contiennent des états et des transitions.  
Un **état** contient des diagrammes de flot, des expressions, des automates.  
Une **transition** contient des gardes, des actions, des propriétés, un "type",
un historique, et peut avoir une priorité.  

L'automate suit la *sémantique synchrone* : à chaque tick, un état est éxécuté
et on passe une transition.  
Au final, un automate est une manière plus simple d'écrire les flots.
D'ailleurs, on
peut traduire l'automate vers des équations de flots.  

![schéma automate](2_05.png)

Il y a trois types de transitions.

### Transition forte

A chaque tick :

- on teste la garde
- si possible, on fait la transition
- on éxécute l'état dans lequel on se trouve (donc après)

Quand il y a **plusieurs transitions sortantes**, il faut leur donner une
**priorité** (priorité basse = faite avant).

**Remarque** : quand on rentre dans un état, il est initialisé. Si on veut
qu'un état ne soit pas réinitialisé (il garde les valeurs de ses variables), il
faut faire une transition *historique*.  
Une transition **forte** ne doit pas regarder les valeurs des variables
actuelles, mais celles au tick précédent.

- ```last 'x``` : donne la dernière valeur de *x* dans tout le système
- ```pre x``` : donne la dernière valeur de *x* dans l'état courant


### Transition faible

Une transition **faible** est comme une transition forte, mais on exécute l'état
courant **avant** la transition, et l'état d'arrivée n'est pas exécuté tout de
suite.

### Transition Synchronisée

Ni faible ni forte, sans garde. Quand tous arrivent vers leur état final,
alors la transition synchronisée se déclenche.

### Modularité

Pour faire des programmes modulaires, et passer des informations entre
modules, on peut :

- imbriquer les automates
- mettre en parallèle deux automates
- partager une mémoire
- envoyer des signaux : un signal peut être émis dans un état ou une transition,
et sont souvent testés dans les gardes
