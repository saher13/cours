### Exemple formalisation
1. Chaque coyote chasse un géocoucou
2. Chaque géocoucou qui dit "beep beep"
3. Aucun coyote n'attrape un géocoucou intelligent
4. Tout coyote qui chasse un géocoucou mais qu ne l'attrape pas est frustré

Conclusion:
5. Si tous les géocoucous disent "beep beep", alors tous les coyotes sont frustrés.

Prédicats :
* coy(x) = x est un coyote
* geo(x) = x est un géocoucou
* int(x) = x est intelligent
* fru(x) = x est frustré
* ch(x, y) = x chasse y
* dbb(x) = x dit "beep beep"
* att(x, y) = x attrape y

1. &forall; x coy(x) &rarr; &exist; y (cha(x, y) &and; geo(y))
2. &forall; x (geo(x) &and; dbb(x) &rarr; int(x))
3. &not(&exist; x &exist; y (coy(x) &and; geo(y) &and; att(x, y) &and; int(y))
4. &forall; x (coy(x) &and; &exist; y (ch(x, y) &and; geo(y) &and; &not;att(x, y)))
5. &forall; x (geo(x) &rarr; dbb(x) &rarr; &forall; z (coy (z) &rarr; fru(z)))





<rule>
<top>
<rule>
<top>
<rule>
<top>
AXIOME
</top>
<bottom>
p(a), &forall; p(x) &#8870; p(a), &exist; x p(x)
</bottom>
<rule>
</top>
<bottom>
&forall; x p(x) &#8870; p(a), &exist; x p(x)
</bottom>
</rule>
</top>
<bottom>
&forall; x p(x) &#8870; &exist; x p(x)
</bottom>
</rule>



<rule>
<top>
<rule>
<top>
<rule>
<top>
AXIOME, mais regle appliquée en dessous pas autorisée.
</top>
<bottom>
p(x) &#8870; p(x)
</bottom>
<rule>
</top>
<bottom>
p(x) &#8870; &forall; x p(x)
</bottom>
</rule>
</top>
<bottom>
&exist; x p(x) &#8870; &forall; x p(x)
</bottom>
</rule>


<rule>
<top>
<rule>
<top>
<rule>
<top>
<rule>
<top>
AXIOME
</top>
<bottom>
p(a) &sequent; p(b), p(a), &exist; x p(x)
</bottom>
</rule>
<rule>
<top>
AXIOME
</top>
<bottom>
p(b) &sequent; p(b), p(a), &exist; x p(x)
</bottom>
</rule>
</top>
<bottom>
p(a) &or; p(b) &sequent; p(b), p(a), &exist; x p(x)
</bottom>
</rule>
</top>
<bottom>
p(a) &or; p(b) &sequent; p(a), &exist; x p(x)
</bottom>
</rule>
</top>
<bottom>
p(a) &or; p(b) &sequent; &exist; x p(x)
</bottom>
</rule>



<rule>
<top>
<rule>
<top>
<rule>
<top>
<rule>
<top>
AXIOME
</top>
<bottom>
p(x), p(y) &#8870; p(y), p(y') &exist; x &forall; y (p(x) &rarr; p(y))
</bottom>
</rule>
</top>
<bottom>
p(x) &#8870; p(y), p(y) &rarr; p(y') &exist; x &forall; y (p(x) &rarr; p(y))
</bottom>
</rule>
</top>
<bottom>
p(x) &#8870; p(y), &exist; x &forall; y' (p(x) &rarr; p(y'))
</bottom>
</rule>
</top>
<bottom>
p(x) &#8870; p(y), &exist; x &forall; y (p(x) &rarr; p(y))
</bottom>
</rule>

### Remarque
On a le droit de renommer les variable liées (uniquement).
