# Vérification - Cours 7

**Note**: ce cours suppose la maitrîse de CTL.

## Interprétation des formules

Pour représenter des ensembles d'états, on va utiliser des formules
booléennes.  
*f ::= x | ¬f | f∨f | ∃x.f*  
On note ⟦f⟧ l'ensemble des interprétations qui
satisfont *f*.  
⟦f⟧ = { v<sup>→</sup>=(v<sub>1</sub>, ..., v<sub>n</sub>)
  | v<sup>→</sup> ⊨ f}  
On prend *f<sub>p</sub>*, l'interprétation des propositions atomiques,
pour chaque *p∈Prop*.  
On note f<sub>δ</sub>(x<sub>1</sub>,...,x<sub>n</sub>,
  x'<sub>1</sub>,...,x'<sub>n</sub>)
 où ⟦f<sub>δ</sub>⟧ =
 {(v<sup>→</sup>, v'<sup>→</sup>) | (v<sup>→</sup>, v'<sup>→</sup>)∈δ}.  
On peut représenter des transitions :  
(0,1) →<sup>t1</sup> (1,0)  
t1 : {x<sub>1</sub>=0 ∧ x<sub>2</sub>=1  
      x'1=x‾<sub>1</sub> ∧ x'2=x‾<sub>2</sub>}

Partant d'un modèle, on veut calculer une représentation de l'ensemble des
états qui satisfont une formule dans ce modèle.

## Transformation

Soit φ∈CTL et τ(φ)∈QBL (Logique Booléenne Quantifiée)  
avec ⟦τ(φ)⟧ = {v<sup>→</sup>∈B<sup>n</sup> | M,v<sup>→</sup>⊨φ}.  
τ(p) = f<sub>p</sub>  
τ(¬φ) = ¬τ(φ)  
τ(φ<sub>1</sub>∨φ<sub>2</sub>) = τ(φ<sub>1</sub>)∨τ(φ<sub>2</sub>)  
τ(∃○φ) = ∃x'<sup>→</sup>. f<sub>δ</sub>(x<sup>→</sup>,x'<sup>→</sup>)
∧ τ(¬φ)[x'<sup>→</sup>/x<sup>→</sup>]  
τ(∀○φ) = ¬τ(¬φ)  
τ(∃◇φ) = pppf(F(x)=φ∨∃○x) (*plus petit point fixe de F(x), l'équation de l'expression*)  
τ(φ<sub>1</sub>∧∃𝔘φ<sub>2</sub>) = idem avec *F(x) = φ<sub>1</sub> ∨ φ<sub>2</sub> ∧ ∃○x*  
τ(∀◇φ) = idem avec F(x) = φ∨∃○x∧∀○x

## Binary Decision Diagram

Les *BDD* peuvent donner une représentation canonique des formules
booléennes.  
Si on a une autre fonction booléenne équivalente, on doit tomber sur le même
objet.  
Exemple (x1=y1)∧...∧(xn=yn) :  
![schéma 1](7_01.png) 
