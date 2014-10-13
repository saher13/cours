# Vérification - Cours 1 : Types de Données Abstraits et Fonctions récursives 

## Manipulation de données 

Les programmes transforment les données. Ils implémentent des fonctions 
entre entrées et sorties.  
Des exemples de domaines : Boolean, Char, Integer, Real, String, List...  
Une fonction a un **type** (domaine et co-domaine) : 
*f : D1 x ... x Dn &rarr; D*.  
Les types doivent être donnés précisément, cela évite beaucoup d'erreurs. 

## Définir des fonctions 

Si le domaine des données est **fini**, on peut énumérer ses valeurs. Ce n'est 
pas toujours pratique, mais en théorie possible.  
Exemple :  
*0 &and; 0 = 0*  
*0 &and; 1 = 0*  
*1 &and; 0 = 0*  
*1 &and; 1 = 1*  
ou, en plus compacte :  
*x &and; y = (if x = 0 then 0 else y)*
Pour écrire des fonctions sur des domaines infinis, on a besoin de 
constructions plus puissantes. Il faut donner une structures aux domaines de 
données infinis. 

## Définition inductive d'ensembles (potentiellement infinis)

Un **élément** (objet) est soit basique, soit construit depuis d'autres 
objets.  
Un **ensemble** est défini par un ensemble de constantes et un ensemble de 
constructeurs.  
Exemple (ensemble *Nat* des entiers naturels) : 
```
Constante : 0 : Nat
Constructeur : s : Nat &rarr; Nat
Exemple d'éléments : 0, s(0), s(s(0)) = s²(0), ...
``` 
Schéma général :  

- Soit un ensemble de constantes C = { c<sub>1</sub> , ... , c<sub>m</sub> }
- Soit un ensemble de constructeurs de la forme *&alpha; : D<sup>n</sup> x A 
&rarr; D*
- L'ensemble des éléments de *D* est le plus petit ensemble tel que : 
 + C &sube; D
 + Pour tout constructeur &alpha;, pour tous d<sub>1</sub>, ... , d<sub>n</sub> 
&isin; D, et tout a &isin; A, alors 
&alpha;(d<sub>1</sub>, ..., d<sub>n</sub>, a) &isin; D

### Le domaine des listes 

Le domaine *List[ * ]* est paramétré par un domaine \* :  

- Constante : *[] : List[ * ]*
- Concaténation gauche : *. : \* x List[ * ] &rarr; List[ * ]*
- Exemples :  
*0 . [] = [0]*  
*[] . [] = [[]] &ne; []*  
*(0 . []) . ((2 . []).[]) = [[0];[2]]*


## Définir des fonctions sur des ensembles définis par induction

Soit *f : Nat &rarr; D*. On définit *f(x)* pour tout *x &isin; Nat*.  
Séparation des cas selon la structure de l'élément :  
*f(0) = ?*  
*(f(s(x)) = ?*  
  
Définition inductive :  
Définir *f(s(x))* en supposant qu'on sait calculer *f(x)*.  
C'est simillaire aux preuves par induction :  
On prouve *P(0)*, et on prouve que *P(s(x))* est vraie, en supposant que 
*P(x)* l'est. 
Schéma général : 

- Soit *f : D x E &rarr; F*
- Pour toute constante *c &isin; D* et tout *e &isin; E*, on définit *f(c,e)* 
comme un élément de *F* 
- Pour tout constructeur *&alpha; : D<sup>n</sup> x A &rarr; D*, pour tout *e 
&isin; E*, 
on définit *f(&alpha;(x<sub>1</sub>, ..., x<sub>n</sub>, a), e)* en utilisant 
*a* et *f(x<sub>1</sub>,e), ..., f(x<sub>n</sub>,e)* 

## Prouver des faits sur les fonctions 

On cherche souvent à prouver ces propriétés : 

- élément neutre : *0 + x = x*
- commutativité : *x + y = y + x*
- associativité : *x + (y + z) = (x + y) + z*
- distributivité : *x * (y + z) = (x * y) + (x * z)*
- idempotence : *Rev(Rev(l)) = l*
- distributivité : *Rev(l<sub>1</sub>@l<sub>2</sub>) 
= Rev(l<sub>2</sub>)@Rev(l<sub>1</sub>)* 

## Résumé 

- La 1ère étape dans la définition d'une fonction est la définition de son 
**type** (domaine et co-domaine)
- Les domaines infinis peuvent être définis par **induction** (partant d'un 
ensemble de constantes, et d'un ensemble de constructeurs) 
- Les fonctions sur des domaines **infinis** sont définies en raisonnant par 
inductions sur la structure des domaines. 
- Les **faits** concernant les fonctions récursives peuvent être prouvés en 
raisonnant sur la structure inductive du domaine. 