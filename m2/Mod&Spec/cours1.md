# Modélisation et spécification - Cours 1 : Systèmes de transition

## Automates

Cette partie n'a été qu'un rappel sur les automates : états, transitions, et 
différentes opérations comme la déterminisation ou la minimisation.  
Se référer à un cours sur les automates. 

## Composition parallèle

Soient S<sub>1</sub> = (Q<sub>1</sub>, A, &delta;<sub>1</sub>) et 
S<sub>2</sub> = (Q<sub>2</sub>, A, &delta;<sub>2</sub>).  
**Sync** &subeq; A est l'ensemble des **actions de synchronisation**.  
S<sub>1</sub> ||<sub>Sync</sub> S<sub>2</sub> 
= (Q<sub>1</sub>xQ<sub>2</sub>)x A x (Q<sub>1</sub>xQ<sub>2</sub>) la plus 
petite relation telle que : 

- q<sub>1</sub> &rarr;<sup>a</sup><sub>&delta;<sub>1</sub></sub> q'<sub>1</sub> 
avec a &notin; Sync
- (q<sub>1</sub>,q<sub>2</sub>) &rarr;<sup>a</sup><sub>&delta;<sub>2</sub></sub>
 (q'<sub>1</sub>,q<sub>2</sub>) et q<sub>2</sub> &rarr;<sup>a</sup><sub>
&delta;<sub>2</sub></sub> q'<sub>2</sub>
avec a &notin; Sync 
  
Une action de synchronisation est une action que tous les systèmes peuvent 
exécuter en même temps.  
