# Modélisation & Spécification - Cours 2 : Traces et simulation

## Sémantique des traces

Soit un système *S = (Q, A, &delta;)*.  

Séquence d'exécution finie : à partir d'un état *q*, c'est une suite de la
forme *q<sub>0</sub> a<sub>0</sub> q<sub>1</sub> a<sub>1</sub> ... 
q<sub>n</sub>* telle que :

- q<sub>0</sub> = q
- &forall;i, 0 &le; i &le; n, on a *q<sub>i</sub> &rarr; 
<sup>a<sub>i</sub></sup><sub>&delta;</sub> q<sub>(i+1)</sub>*

Une séquence est maximale ssi q<sub>n</sub> &#8603; (&#8708;q' &isin; 
Q q<sub>n</sub> &rarr;<sup>a</sup> q').
Toutes les séquences infinies sont maximales.
On voit ça comme une suite " état - action - état - action..."

La trace d'une séquence d'exécution est sa projection sur A\{&Tau;} (on ne 
garde que les actions non-invisibles (voir plus loin) ).  
Trace (q) = {traces des séquences d'exécution maximales à partir de q}

Equivalence des traces :  
q &equiv;<sub>T</sub> q' ssi  Traces(q) = Traces(q').  
Inclusion des traces :  
q &sube;<sub>T</sub> q' ssi Traces(q) &sube; Traces(q').  
Toutes les traces permises par l'implémentation doivent être admises par la
spécification.

## (Bi)simulation

La relation binaire entre Q et Q, R &sube; Q x Q, est une bisimulation ssi  
&forall;q<sub>1</sub>,q<sub>2</sub>&isin;Q,  q<sub>1</sub> R q<sub>2</sub> 
&hArr;
&forall;q'<sub>1</sub>,&isin;Q, &forall;a&isin;A  q<sub>1</sub> 
&rarr;<sup>a</sup><sub>&delta;</sub> q'<sub>1</sub> 
&rArr; &exist;q'<sub>2</sub> &isin; Q, 
 q<sub>2</sub> &rArr;<sup>a</sup><sub>&delta;</sub> q'<sub>1</sub> R 
q'<sub>2</sub>  
Autrement dit :   
Deux automates sont bisimulés si à chaque fois que l'un fait quelque chose 
(une action a), l'autre peut lui répondre avec une action similaire (et 
vice-versa). Les deux états ont la même capacité à évoluer.  
Les deux graphes sont alors équivalents / similaires.  
Donc si l'autre ne peut pas toujours répondre par la même action, ils ne sont
pas équivalents.  
La simulation c'est quand ça marche dans un sens.

- q<sub>1</sub> &equiv;<sub>bisim</sub> q<sub>2</sub> 
ssi &exist; R une bisimulation telle que q<sub>1</sub> R q<sub>2</sub>
- q<sub>1</sub> &sube;<sub>sim</sub> q<sub>2</sub> 
ssi &exist; R une simulation telle que q<sub>1</sub> R q<sub>2</sub>  

Alors q<sub>2</sub> simule q<sub>1</sub> et q<sub>1</sub> raffine q<sub>2</sub>.

## Action invisible

&Tau; &isin; A est une action invisible.
Si un des systèmes peut, sans prévenir, faire une séquence d'actions invisibles
(éventuellement nulle : &Tau;<sup>*</sup> ) et arriver à un état, l'autre doit 
aussi pouvoir faire une séquence d'actions invisibles (pas forcément la même) 
pour arriver au même état.  
C'est symétrique pour la bisimulation. 
