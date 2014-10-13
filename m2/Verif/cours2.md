# Vérification - Cours 2 : Spécification de programmes par la logique

## Spécification abstraite d'une fonction 

On considère une fonction *f: Dom &rarr; CoDom*.  
Comment décrire son comportement de manière abstraite (càd sans donner de 
détails d'implémentation).  
On définit la spécification comme une relation *Spec_f* entre les entrées et 
les sorties de *f* : *Spec_f(In,Out) &sube; Dom x CoDom*. On cherche un 
formalisme naturel pour décrire une telle relation.  

## Domaines d'interprétation 

On définit le **domaine de données** avec un ensemble d'opérations et des 
prédicats :  

- on considère un domaine *D*
- soit *Op* un ensemble d'opérations, interprétées comme des fonctions sur *D*
- soit *Pred* un ensemble de prédicats interprétés comme des relations sur *D*

Remarque : Ici, *Op* pourrait inclure des constantes, vues comme opérateurs 
d'arité 0.  
  
Le **domaine d'interprétation** est un triplet *(D, Op, Pred)*. 

### Exemples de domaines d'interprétation

- (Bool, {tt, ff, not, or, and}, {=})
- (Nat, {0,s,+}, {<})
- (List[ * ], {[],.,@}, {=}) 

## La logique du 1er ordre sur les domaines de données

- Soit *(D, Op, Pred)* un domaine d'interprétation
- Soit *Var* un ensemble de variables
- On définit les termes comme *t ::= v &isin; Var | op(t,...,t)* 
avec *v &isin; Var* et *op &isin; Op*.  

Les termes sont interprétés comme des éléments du domaine *D* : 
- soit *v &isin; Var &rarr; D* une valuation des variables
- alors, *(t)<sub>y</sub>* est la value de *D* obtenue par évaluation de *t* 
en utilisant *v* comme valuation

## Syntaxe des formules 

Une formule est de la forme 
*&Phi; ::= T | &perp; | p(t1,...,tn) | &Phi; &or; &Phi; | &Phi; &and; &Phi; 
| &exist; v.&Phi; | &forall;v. &Phi; | &Phi; &rArr; &Phi;* 
avec *p &isin; Pred* et *v &isin; Var*.  
Abbréviations : 

- &not;&Phi; ::= (&Phi;&rArr;&Perp;)
- &Phi; &lArr;&rArr; &Phi;' ::= &Phi; &rArr; &Phi;' &and; &Phi;' &rArr; &Phi;

Un occurence de la variable *x* est liée dans une formule &Phi; si elle 
est sous un **quantificateur** &exist;x ou &forall;x. On ne considère que des 
formules où toutes les occurences d'une variables sont soient liée, soient non 
liées. Une variable est **libre** dans &Phi; si toutes ses occurences dans 
&Phi; ne sont pas liées. Une formule est **close** si elle n'a pas de 
variables libres.  

## Sémantique des formules 

Soit une valuation *v : Var &rarr; D* des variables libres, on peut définir une 
interprétation &Phi;[v] qui remplace toute occurence d'une variable libre par 
ses valeurs associées : 

- x[v] = v(x)
- p(t1,...,tn)[v] = p(t1[v], ..., tn[v])
- (&Phi; &and; &Phi;')[v] = &Phi;[v] &or; &Phi;'[v]
- (&Phi; &or; &Phi;')[v] = &Phi;[v] &or; &Phi;'[v] 
- (&forall;v.&Phi;)[v] = &forall;v. &Phi;[v]
- (&exist;v.&Phi;)[v] = &exist;v. &Phi;[v]
- (&Phi; &rArr; &Phi;')[v] = &Phi;[v] &rArr; &Phi;'[v]

Donnée une valuation *v*, on dit que *v* **satisfait** &Phi; ssi &Phi;[v] est 
vraie, càd quand interprétant la formule avec *v*, la formule est valide.  
Les formules sont interprétées comme des relations sur *D*, càd les ensembles 
de valuations des variables qui satisfont la formule.  
Soit **[&Phi;]** l'ensemble des valuations *v* qui satisfont &Phi;. Une 
formule est **valide** si elle est satisfaite par toutes les valuations. Une 
formule est **satisfaisable** si il existe une valuation qui la satisfait.  
  
Remarque : Les formules closes sont soit valides, soit contradictoires. Leur 
valeur ne dépend pas de la valuation des variables. Soit toute valuation les 
satisfont, soit aucune. 

## Prouver la validité 

Pour montrer la validité d'une formule quantifiée, il faut la **prouver 
formellement** : 

- *p(t<sub>1</sub>, ..., t<sub>n</sub>)* : par hypothèse, ou définition de *p*
- &not;&Phi; : on suppose &Phi;, on prouve par contradiction (&perp;)
- &Phi; &or; &Phi;' : on prouve &Phi; ou on prouve &Phi;'
- &Phi; &and; &Phi;' : on prouve &Phi; et &Phi;'
- &exist;v.&Phi; : on fourni un témoin *t* pour *v*, et on montre 
&Phi;[v &rarr; t]
- &forall;v.&Phi; : on suppose *v* et on montre &Phi;
- &Phi; &rArr; &Phi;' : on suppose une hypothèse *H : &Phi;* et on montre &Phi;'

## Logique à plusieurs domaines 

En général, on a besoin de raisonner sur plusieurs domaines simultanément. On 
considère alors des domaines d'interprétation de la forme 
*(D<sub>1</sub>, ..., D<sub>n</sub>, Op, Rel)* où les opérations et relations 
sont définies sur un ou plusieurs domaines *D<sub>1</sub>, ..., D<sub>n</sub>*. 

## Multi-ensembles

- On donne le domaine des multi-ensembles : *Multiset[ * ] = * &rarr; Nat*. 
- Les opérations sur les multi-ensembles : 

 - &empty; : Multiset[ * ] = &lambda;x &isin; *. 0
 - Sg : \* &rarr; Multiset[ * ] = Sg(a) = &lambda;x &isin; \*. 
if x = a then 1 else 0
 - &omega; : Multiset[ * ] x Multiset[ * ] &rarr; Multiset[ * ].  
   M<sub>1</sub> &omega; M<sub>2</sub> = &lambda;x &isin; * . M<sub>1</sub>(x) 
+ M2(x)
- Propriétés : élément neutre (&empty;), commutativité, associativité. 

## Prédicats inductifs 

Voir cours sur l'induction

## Résumé 

Les spécifications sont des définitions abstraites de l'effet des fonctions. 
Aucun détail d'implémentation n'est imposé.  
La logique est un langage naturel pour la description abstraite des relations 
entrées-sorties.  
L'abstraction permet une conception modulaire : l'utilisateur n'a besoin que 
de la spécification, et l'implémenteur doit s'assurer que le spécification est 
satisfaite.  
Il peut y avoir différentes manières d'exprimer la même spécification, en 
utilisant les prédicats récursifs ou inductifs. 