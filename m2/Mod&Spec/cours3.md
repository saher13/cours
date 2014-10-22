# Modélisation & Spécification - Cours 3

La spécification peut-être décrite par un comportement abstrait, ou un langage 
logique qui permet de parler de l'évolution des états. 

## Logique temporelle propositionnelle linéaire (LTL)

Soit *Prop* un ensemble de propositions atomiques. 

&phi; ::= p &isin; Prop | &not;&phi; | &phi; &or; &phi; | &#25CB;&phi; (next) 
| &phi; &#1C62; &phi; (until)  
*&#25CB;&phi;* : dans l'état suivant, &phi; sera vraie.  
*&phi;<sub>1</sub> &#1C62; &phi;<sub>2</sub>* : on veut &phi;<sub>2</sub> à un 
moment (futur ou maintenant). Et toutes les états avant celui-ci doivent 
satisfaire &phi;<sub>1</sub>.  
  
Une séquence d'exécution est un fonction &sigma; qui associe à chaque instant 
une certaine interprétation des propriétés atomiques.  
&sigma; : &#2116; &rArr; &weierp; (Prop).  
&sigma; (i) : les propriétés atomiques qui sont vraies au point *i* de la 
séquence &sigma; 

- &sigma;,i &#22A8; &phi; (satisfait)  
- &sigma;,i &#22A8; p  ssi  p &isin; &sigma;(i)  
- &sigma;,i &#22A8; &not;&phi;  ssi  &sigma;,i &22AD;  &phi;  
- &sigma;,i &#22A8; &phi;<sub>1</sub> &or; &phi;<sub>2</sub>  
ssi  &sigma;,i &22AD; &phi;<sub>1</sub> ou &sigma;,i &22AD; &phi;<sub>2</sub>  
- &sigma;,i &#22A8; &#25CB;&phi; ssi &sigma;,i+1 &22A8; &phi;  
- &sigma;,i &#22A8; &phi;<sub>1</sub> &1C62; &phi;<sub>2</sub>  
ssi  &exist; j &ge; i (&sigma;,j &22AD; &phi;<sub>2</sub> et &forall;k, 
i&le;k<j &rArr; &sigma;,k &22AD; &phi;<sub>1</sub>)  

Notation : on écrit &sigma; &#22AB; &phi; &equiv; &sigma;,0 &#22AB; &phi; 
(état initial)
Un **système** est de la forme *M=(Q, &pi;, &delta)* avec : 
- Q l'ensemble des états
- &pi; : Q &rarr; &weierp;(Prop)
- &delta; &sube; Q x Q

Traces(M,q) = {&pi;(q<sub>0</sub>)&pi;(q<sub>1</sub>)...
|q<sub>0</sub>q<sub>1</sub>...une séquence d'exécution de M à partir de q}
M,q &#22A8; &phi; ssi &forall;&delta;&isin;Traces(q), &sigma; &#22A8;&phi;


On définit *&#25C7;&phi; = vrai &#1C62; &phi;* : inévitablement, &phi; sera 
vraie dans le futur.  
On définit *&#25A1;&phi; = &not;&#25C7;&not;&phi; : &phi; sera tout le temps 
vraie (à partir de maintenant).  


## Exclusion mutuelle

![schema3_01.png](schema3_01.png)

Att1 &and; Att2 &rArr; &#25A1; (&not;Util1 &or; &not;Util2)  
Relachement de la ressource : 
&#25A1; (Util1 &rArr; &#25C7;Att1;) (toujours, à chaque fois qu'on est dans 
l'état d'utilisation, il existe un point tel qu'on attend, donc qu'on aura 
rendu)
Garantie d'obtention de la ressource : 
&#25A1;(Att1 &rArr; &#25C7;Util1) et &#25A1;(Att2 &rArr; &#25C7;Util2) 

**Propriété de sûreté** : toute action faite laisse le système dans un ensemble 
d'états "bons" ("Il n'arrive jamais quelque chose de mauvais").  
**Propriété de vivacité** : "quelque chose de bon va arriver".  

Dans le système donné, rien ne garantit l'exclusion mutuelle. On introduit un 
système pour la ressource : 
![schema3_02.png](schema3_02.png)
Ensuite on synchronise :  

- donner1 &harr; obtenir1
- donner2 &harr; obtenir2
- recupérer1 &harr; rendre1
- recupérer2 &harr; rendre2

## Propriétés temporelles

Infiniment souvent *p* : à chaque étape de la séquence, on a *p* = 
*&#25A1;&#25C7;&phi;*.  
Nombre fini de *p* : *&#25C7;(&#25A1;&not;&phi;)* 
&hArr; *&not;(&#25A1;&#25C7;&phi;)*.  
  
On peut raisonner sur la **futur** : "si *p* alors *q* sera vraie" est une 
propriété de **réponse** (&#25A1;(p&rArr;&25C7;q)).  
On peut aussi raisonner sur le **passé** : "si *q* alors *p* a été vraie.  