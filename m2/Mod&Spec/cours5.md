# Modélisation & Spécification - Cours 5 : Modèles

## Réseaux de Petri

On veut mettre en parallèle des réseaux. On met des jetons dans les états.
Pour faire une action synchronisée, il faut que les états possèdent au moins 1
jeton. Les jetons sont transférés à l'état d'arrivée.  
![schéma 1](5_01.png)

Un **réseau de Petri** est un triplet R = (P, T, &Delta;) avec

- P un ensemble fini de places
- T un ensemble fini de transitions
- &Delta; &sube; P&#10761;T &cup; T&#10761;P
- P&cap;T = &empty;

Soit t&isin;T :  
<sup>&#9679;</sup>t = { p&isin;P | (p,t)&isin;&Delta }  
t<sup>&#9679;</sup> = { p&isin;P | (t,p)&isin;&Delta }

On définit le **marquage** M : *P&rarr;N*. La relation de transition entre
deux marquages M<sub>1</sub> et M<sub>2</sub> est la relation
M<sub>1</sub> &#9655;<sub>t</sub> M<sub>2</sub> telle que

- &forall;p&isin;<sup>&#9679;</sup>t, m<sub>1</sub>(p)&ge;1
- &exist;M &forall;p&isin;P, M(p) = M<sub>1</sub>(p) - 1 si
p&isin;<sup>&#9679;</sup>t, ou M<sub>1</sub>(p) sinon
- &forall;p&isin;P, M<sub>2</sub>(p= = M(p)+1 si p&isin;t<sup>&#9679;</sup>,
ou M<sub>2</sub> sinon

On a &#9655; = &cup;<sub>t&isin;T</sub> &#9655;<sub>t</sub>

On définit M(R), l'ensemble des **marquages possibles** du réseau *R*
(càd [P&rarr;&#8469;]).  
(M(R),&#9655;) est le **graphe de marquage**.  
Acc(R,M) est l'ensemble des **marquages accessibles** dans *R* à partir de *M*.

En pratique, on fait un n-uplet (où *n* est le nombre d'états), et à chaque
*i* de ce tuple, on inscrit le nombre de jetons sur l'état *i*.  

*R* est **k-borné** si à partir d'un marquage M, tous les marquages
accessibles ont toujours *k* jetons ou moins : &forall;M'&isin;Acc(R,M),
&forall;p&isin;P, on a M'(p)&le;k

On peut assigner des **priorités** avec La relation d'ordre &#8828;
&sube; T&#10761;T.  
M<sub>1</sub> &#9655;<sup>&#8828;</sup><sub>t</sub> M<sub>2</sub> ssi
M<sub>1</sub> &#9655;<sub>t</sub> M<sub>2</sub> et
&forall;t'&isin;T, t&#9655;t' &rArr;
&not;M<sub>1</sub>&#9655;<sub>t</sub>M<sub>2</sub>

On peut faire des **arcs inhibiteurs** : il faut que l'état soit marqué avec
0 (en pratique, on met un 0 sur l'arc, du côté de la garde de la transition)
