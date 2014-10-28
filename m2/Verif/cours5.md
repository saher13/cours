# Vérification - Cours 5 : Preuves de terminaison de programmes

On veut vérifier qu'un programme termine, quelque soient ses entrées.  

```
f : Nat;
ifact (n : Nat) =
  i : Nat;
  f := 1;
  i := 0;
  while i = n do
    i := i + 1;
    f := i ∗ f;
```

A chaque itération, *une quantité décroit*. Cette quantité doit être définie 
comme *une fonction de l'état du programme*.

## Relations bien fondées : produit et ordre

```
Rappel : 
une relation est bien fondée quand il n'y a pas de chaine infinie 
descendante. Il y a forcément un élément minimum. 
```
Soient *(E<sub>1</sub>,&#8826;<sub>1</sub>), ..., 
(E<sub>n</sub>,&#8826;<sub>n</sub>)* *n* ensembles bien fondés.  
On définit le produit d'une relation bien fondée :  
&#8826;<sub>&#10761;</sub>&sube;(E<sub>1</sub>&#10761; ... 
&#10761;E<sub>n</sub>)²  
(e<sub>1</sub>, ..., e<sub>n</sub>) &#8826;<sub>&#10761;</sub> 
(e'<sub>1</sub>, ..., e'<sub>n</sub>) &hArr; &forall;i&isin;{1, ..., n}. 
e<sub>i</sub>&#8826;<sub>i</sub>e<sub>i'</sub>  

On définit l'ordre lexicographique d'une relation bien fondée :  
&#8826;<sub>l</sub>&sube;(E<sub>1</sub>&#10761; ... 
&#10761;E<sub>n</sub>)²  
(e<sub>1</sub>, ..., e<sub>n</sub>) &#8826;<sub>l</sub> 
(e'<sub>1</sub>, ..., e'<sub>n</sub>) &hArr; &exist;i&isin;{1, ..., n}. 
(e<sub>i</sub>&#8826;<sub>i</sub>e<sub>i'</sub> &and; (&forall;j < i . 
e<sub>j</sub> = e<sub>j'</sub>)) 

## Fonction de rang 

Soit X = {x1, ..., xn} l'ensemble des variables du programme.  
Si on considère la boucle ```while C do S```, on pose &Phi; comme l'invariant 
de la boucle, soit &forall;&mu;,&mu;'.(&mu;&#8872;&Phi;&and;C and 
&mu;&rarr;<sup>s</sup>&mu;') &rArr; &rho;(&mu)&#8826;&rho;&mu;'  
&rho;:D<sup>n</sup>&rarr;E est la fonction de rang de la boucle, telle que 
&forall;&mu;,&mu;'.(&mu;&#8872;&Phi;&and;C and &mu;&rarr;<sup>s</sup>&mu;') 
&rArr &rho;(&mu);&#8872;&rho;(&mu;') avec (E,&#8872;) un ensemble bien fondé.  
Terminaison : *Une boucle **while** termine si **S** est une déclaration 
terminale et que la boucle a une fonction de rang.*


## Vérifier la correction totale 

{|&Phi;|}S{|&psi;|} ssi &forall;&mu;.(&mu;&#8826;&Phi;&rArr;&exist;&mu;'.
(&mu;&rarr;<sup>s</sup>&mu;'&and;&mu;'&#8826;&psi;))  
Depuis un état satisfaisant *&Phi;*, l'exécution de *S* termine et amène à un 
état satisfaisant &psi;

## Règle de la correction totale 

On rajoute une règle à la correction partielle : page 8 
On prend en compte l'invariant de la boucle, &Phi;, et on constate que la 
fonction de rang donne une valeur inférieur. 
![schéma p8](5_01.png)

## Exemples

## Résumé 

**Correction totale = correction partielle + terminaison.**  
La correction partielle assure que le programme fournit les résultats attendus 
si il termine.  
Pour prouver la terminaison, on doit raisonner sur le "bien-fondement" des 
calculs.  
Cela se calcule en trouvant des fonctions de rang pour les boucles, qui font 
correspondre les états à des éléments d'ensembles bien fondés.  
Plusieurs ensembles bien fondés peuvent être considérés. En particulier, on a 
souvent besoin de relations d'ordre lexicographique bien fondées.  
En général, les preuves de terminaisons ne peuvent pas être automatisées, 
même si certaines techniques incomplètes existent sur des cas particuliers.  