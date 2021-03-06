<link href="../github_md.css" rel="stylesheet"></link>
### Rappels
* (1) : p(x) : valuation &sigma; est important, valeur formule dépend de &sigma;
* (2) : &forall; x p(x) : valuation  &sigma; pas prise en compte (variable liée)

I : Interpretation
* D = {0, 1}
* I(p) = {0} : p vrai pour valeur 0

I satisfait p(x) ?  
Oui avec &sigma;[x := 0]  
I n'est pas un modèle de p(x)
I satisfait &forall; p(x) ?  
Non, &sigma; n'est pas pris en compte, et p(1) faux

&forall; x p(x) est satisfaisable (avec I(p) = {0, 1} par exemple)

p(x) &and; &not;p(x) est insatisfaisable  
&forall x p(x) &and; &exist;y &not;p(z) non plus

### Cloture universelle
p(x) est une formule, &forall; x p(x) est sa cloture universelle.  
(&forall; est appelé quantificateur universel)  
(&exist; est appelé quantificateur existentiel)

&not;p(x) &or; p(x) est valide.

### Slide 28
&forall; x &exist; p(x,y) &#8877; &exist; y &forall;
 x p(x,y)
Interpretation I :
* D = {0, 1}
* I(p) = {(0, 0), (1, 1)}
&forall; &sigma; :  
[&forall; x &exist; y p(x, y)]I,&sigma; = VRAI
> [&exist; y p(x, y)]I,&sigma;[x := 0] . [&exist; y p(x, y)]I,&sigma;[x := 0]  
(3) = ([p(x,y)]I,&sigma;[x := 0][y := 0] + [p(x,y)]I,&sigma;[x := 0][y := 0]) . (...)

>I(p)([x]I,&sigma;[x := 0][y := 0], [y]I,&sigma;[x := 0][y := 0])
= I(p)(0, 0) = VRAI

> (3) = (VRAI + FAUX) . (FAUX + VRAI) = VRAI

(4) [&exist; y &forall; x p(x, y)]I,&sigma; = FAUX
> [&exist; y &forall; x p(x, y)]I,&sigma;
= [&forall; x p(x, y)]I,&sigma;[y := 0] + [&forall; x p(x, y)]I,&sigma[y := 1];  
= ([x p(x, y)]I,&sigma;[y := 0][x := 0] . [x p(x, y)]I,&sigma;[y := 0][x := 1])  
+ ([x p(x, y)]I,&sigma;[y := 1][x := 0] . [x p(x, y)]I,&sigma;[y := 1][x := 1])  
= (VRAI . FAUX) + (FAUX . VRAI) = FAUX  

I rend (3) VRAI et (4) FAUX, (3) &#8877; (4)

### Slide 28
&exist; x (A &and; B) &#8872; &exist; x A &and; &exist; x B

Supposons [&exist; x (A &and; B)]I,&sigma; = VRAI pour I et &sigma;  
&rArr; &Sigma;(d &isin; D)[A &and; B]I,&sigma;[x := d] = VRAI  
&rArr; &Sigma;(d &isin; D)[A]I,&sigma;[x := d] = VRAI &and; &Sigma;(d &isin; D)[B]I,&sigma;[x := d] = VRAI  
&rArr; [&exist; x A]I,&sigma; = VRAI &and; [&exist; x B]I,&sigma; = VRAI  


### Slide 28

&exist; x (A &and; B) &#8877; &exist; x A &and; &exist; x B  
interpretation I
* D = {0, 1}
* I(p) = {0}
[&exist; p(x) &and; &exist; &not;p(x)]I,&sigma;] = VRAI  
[&exist; (p(x) &and; &not;p(x))]I,&sigma;] = FAUX

### Slide 30 - Précision sur le parenthésage
&exist; x (B &rarr; A) &equiv; (&forall; x B) &rarr; A

### Slide 34

* OK :  
  &Delta; &#8870; &forall; x &exist; y p(x, y)  
  - - -  
  &Delta; &#8870; &exist; y p(a, y)
* KO :  
  &Delta; &#8870; &forall; x &exist; y p(x, y)  
  - - -  
  &Delta; &#8870; &exist; y p(y, y)

### Slide 35

p(a) &#8870; p(a)   AXIOME  
- - - (&exist; i)  
p(a) &#8870; &exist; x p(x)  
- - -  
&#8870; p(a) &rarr; &exist; x p(x)


p(a) &or; p(b) &#8870; p(a)   ECHEC (pareil avec b à la place de a)
- - -  
p(a) &or; p(b) &#8870; &exist; x p(x)

AXIOME    AXIOME    AXIOME
- - -  
AXIOME   p(a) &or; p(b), p(a) &#8870; p(a)    p(a) &or; p(b), p(b) &#8870; p(b)
- - -  
p(a) &or; p(b) &#8870; p(a) &or; p(b)     p(a) &or; p(b), p(a) &#8870; &exist; x p(x)    p(a) &or; p(b), p(b) &#8870; &exist; x p(x)
- - -  (&or;e)
p(a) &or; p(b) &#8870; &exist; x p(x)


### Exemple

### Preuve facile :  
&forall; x p(x) &#8870; &forall; x p(x)  
- - - (&forall;x e){x &larr; x}  
&forall; x p(x) &#8870; x p(x)  
- - - (&exist;x i)  
&forall; x p(x) &#8870; &exist; x p(x)

### Preuve FAUSSE :
AXIOME    AXIOME  
- - -  
&exist; x p(x) &#8870; &exist; x p(x)     &exist; x p(x), p(x) &#8870; p(x)  
- - - (&exist;x i)  
&exist; x p(x) &#8870; x p(x)   
- - - (&forall;x i)  
&exist; x p(x) &#8870; &forall; x p(x) 

Ici, (&exist;x i) ne doit pas être appliquée car x EST LIBRE

###


&exist; y &forall; x A &#8870; &exist; y &forall; x A    &exist; y &forall; x A, &forall; x A &#8870; A    ECHEC car y libre (à droite)  
- - - (&exist; x e)  
&exist; y &forall; x A &#8870; A  
- - - (&exist; x i)  
&exist; y &forall; x A &#8870; &exist; y A  
- - - (&forall; x i)  
&exist; y &forall; x A &#8870; &forall; x &exist; y A



AXIOME    &exist; y &forall; x A, &forall; x A &#8870; &forall; A  
- - - AXIOME     (&forall;x e)  
AXIOME    &exist; y &forall; x A, &forall; x A &#8870; A  
- - -  AXIOME (&exist;x i)  
&exist; y &forall; x A &#8870; &exist; y &forall; x A    &exist; y &forall; x A, &forall; x A &#8870;  &exist; y A  
- - - (&exist; x e)  
&exist; y &forall; x A &#8870; &exist; y A  
- - - (&forall; x i)  
&exist; y &forall; x A &#8870; &forall; x &exist; y A

AXIOME    &exist; x (A &and; B), (A &and; B) &#8870; A &and; B (...)  
- - - AXIOME (&and; e) (...)  
AXIOME     &exist; x (A &and; B), (A &and; B) &#8870; A  (...)
- - - AXIOME (&exist;x i) (...)  
&exist; x (A &and; B) &#8870; &exist; x (A &and; B)    &exist; x (A &and; B), (A &and; B) &#8870; &exist; x A    (...)  
- - - (&exist;x e) (...)  
&exist; x (A &and; B) &#8870; &exist; x A    &exist; x (A &and; B) &#8870; &exist;x B  
- - - (&and; i)  
&exist; x (A &and; B) &#8870; &exist; x A &and; &exist;x B

<r>
  <t>
    <r>
      <t>
        <r>
      	  <t>
	    
      	  </t>
      	  <b>
	    &exist; x (A &and; B) &#8870; &exist; x (A &and; B)
	  </b>
    	</r> 
        <r>
      	  <t>
	    <r>
	      <t>
	        &exist; x (A &and; B), (A &and; B) &#8870; A &and; B
	      </t>
	      <b>
	      &exist; x (A &and; B), (A &and; B) &#8870; A
	      </b>
	    </r>
      	  </t>
      	  <b>
	    &exist; x (A &and; B), (A &and; B) &#8870; &exist; x A
	  </b>
    	</r>      
      </t>
      <b>
        &exist; x (A &and; B) &#8870; &exist; x A
      </b>
    </r>
    <r>
      <t>(...)</t>
      <b>&exist; x (A &and; B) &#8870; &exist;x B</b>
    </r>
  </t>
  <b>&exist; x (A &and; B) &#8870; &exist; x A &and; &exist;x B</b>
</r>