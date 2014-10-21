# Vérification - Cours 3 : Preuves de programmes récursifs

## Implémentation vs Spécification 

On suppose qu'on veut définir *f : Dom &rarr; CoDom*.  
On considère une spécification abstraite *Spec_f(In, Out) &sube; Dom x CoDom.* 
Soit *Impl_f* une implémentation de *f* (càd une fonction récursive). 
L'implémentation *Impl_f* satisfait la spécification *Spec_f* ssi : 
&forall;In&isin;Dom, &forall;Out&isin;CoDom . 
(Impl_f(In) = Out) &rArr; Spec_f(In,Out)  
ou, de manière équivalente :  
&forall;In&isin;Dom &rArr; Spec_f(In, Impl_f(In))
Toute entrée de la spécification est vérifiée, et le résultat de 
l'implémentation appliquée sur cette entrée est la spécification : la 
correction est toujours définie par rapport à une spécification donnée. 

## Preuve de correction par induction 

On doit vérifier chacune des propriétés. 

## Fonction de tri 

Ms(l) = Multi-ensemble contenant les éléments de *l*.  
Ms(l1) = Ms(l2) => l2 est une permutation de l1. 

## Relations bien fondées 

Soit E un ensemble, et soit *&#8826; &sube; E x E* une relation binaire sur E.  
La relation &#8826; est **bien fondée** si elle n'a pas de chaine descendante 
infinie, càd aucune séquence de la forme  
e<sub>0</sub> &#8827; e<sub>1</sub> &#8827; ...  
(E. &#8826;) est alors un ensemble **bien fondé**.  
*Théorème* : &#8826; est bien fondée ssi 
&forall;F&sube;E.F&#8826;&ne;&#8826;empty; 
&rArr; (&exist;e&isin;F . &forall;e'&isin;F . e' (&#8836;) e)

## Induction Noetherienne 

Soit *(E, &#8826;)* une relation bien fondée et soit la mesure 
*&rho; : D &rarr; E*.  
Soit &#8826;<sub>&rho;</sub> &sube; D x D une relation telle que :  
x &#8826;<sub>&rho;</sub> t &hArr; &rho;<sub>x</sub>&#8826;&rho;<sub>y</sub>.  
Règle d'induction :  
&forall; x &isin; D . 
((&forall;y.y &#8826;<sub>&rho;</sub> x &rArr; P(y)) &rArr; P(x))
<hr/>
&forall;x&isin;D . P(x)

## Conclusion 

Les spécifications donnent une description abstraite d'une fonction, sous la 
forme d'une formule logique. On ne donne pas de détails d'implémentation : on 
peut même donner plusieurs fonctions d'implémentation.  
On peut séparer la vérification en différents sous-modules. On raisonne dans 
chaque fonction par rapport aux spécifications qu'on utilise. 