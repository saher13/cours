# Modélisation & Spécification - Cours 4 : CTL

## Equivalence  

&#10214;&phi;&#10215; : ensemble des traces qui satisfont &phi;
q'&#8849;q : Traces(q')&sube;Traces(q)  
q&#8872;&phi; : Traces(q)&sube;&#10214;&phi;&#10215;  
q'&#8849;q &and; q&#8872;&phi; &rArr; q'&#8872;&phi;
soit Traces(q')&sube;&#10214;&phi;&#10215;, soit q'&#8872;&phi;  

q&asymp;q' &rArr; &forall;&phi;, q&#8872;&phi; &hArr; q'&#8872;&phi;
q&#8772;q' &rArr; &exist;&phi;, q&#8872;&phi; et q'&#8877;&phi;

## CTL : Logique Temporelle Arborescente

Dans *LTL*, on fixait une séquence, et on raisonnait toujours sur cette
séquence. Ici, on peut bifurquer sur tous les chemins. On introduit des
quantificateurs : les formules *LTL* sont implicitement précédées de &forall;.  
&phi; ::= p&isin;Prop | &not;&phi; | &phi;&or;&phi; | &exist;&#9675;&phi; |
&forall;&#9675;&phi; | &phi;&exist;&#120088;&phi; |
&phi;&forall;&#120088;&phi;  
On définit un ensemble d'états *M = (Q, &Pi;, &delta;)*, avec
*&Pi;:Q&rarr;&weierp;(Prop)*.  

- M,q&#8872;p ssi p&isin;&Pi;(q)  
- M,q&#8872;&not;&phi; ssi M,q&#8877;&phi;
- M,q&#8872;&phi;<sub>1</sub>&or;&phi;<sub>2</sub>
ssi M,q&#8872;&phi;<ubs>1</sub> ou M,q&#8872;&phi;<ubs>2</sub>  
- M,q&#8872;&exist;&#9675;&phi; ssi &exist;q', q&rarr;<sub>&delta;</sub>q'
&and; M,q'&#8872;&phi;  
- M,q&#8872;&forall;&#9675;&phi; ssi &forall;q', q&rarr;<sub>&delta;</sub>q'
&and; M,q'&#8872;&phi;  
- M,q&#8872;&phi;<sub>1</sub>&exist;&#120088;&phi;<sub>2</sub>
ssi &exist;q<sub>0</sub>q<sub>1</sub>...&isin;Seq_Exec(M,q),
&exist;j&ge;0. M,q<sub>j</sub>&#8872; et &forall;0&le;i&le;j,
M,q<sub>i</sub>&#8872;&phi;<sub>1</sub>  
(on rappelle que *Seq_Exec(M,q)* = q<sub>0</sub> initial et &forall;i&ge;0
q<sub>i</sub>&rarr;<sub>&delta;</sub>q<sub>i+1</sub>).  
- M,q&#8872;&phi;<sub>1</sub>&forall;&#120088;&phi;<sub>2</sub>
ssi &forall;q<sub>0</sub>q<sub>1</sub>...&isin;Seq_Exec(M,q),
&exist;j&ge;0. M,q<sub>j</sub>&#8872; et &forall;0&le;i&le;j,
M,q<sub>i</sub>&#8872;&phi;<sub>1</sub>

&exist;&#9671;&phi; = vrai&exist;&#120088;&phi; (&phi; est **possible** un
jour : *accessibilité*).  
&forall;&#9633;&phi; = &not;&exist;&#9671;&not;&phi; : il n'existe pas de
chemin sur lequel il y a &not;&phi; (donc &phi; est **toujours** vraie).  
&forall;&#9671;&phi; = vrai&forall;&#120088;&phi; (&phi; est **inévitable**
quelque soit le chemin).  
&exist;&#9633;&phi; = &not;&forall;&#9671;&not;&phi; (il existe une séquence
dans laquelle &phi; est toujours vraie).  
