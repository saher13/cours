# XML - Cours 3 : DTD

Une **DTD (Document Type Definition)** définit la structure d'un document XML. 
Elle regroupe des contraintes à respecter pour qu'un document soit *valide* : 
les éléments pouvant apparaître, l'ordre, la présence de texte, les attributs 
(autorisés ou obligatoires).  
Les DTD sont simples à utiliser mais limitées : on les préfère pour de petits 
modèles. Les schémas (cours 5) sont plus modulaires et plus adaptés aux gros 
modèles. 

## Déclaration 

Elle doit être placée dans le prologue : ```<!DOCTYPE root-element ... >```. 

### DTD interne 

Une DTD est **interne** si elle est directement incluse dans le document : 
```
<!DOCTYPE simple [ 
  <!ELEMENT simple (#PCDATA)> 
]>
```
Ici, le nom de l'élément racine est "*simple*", et il ne peut contenir que du 
texte (*PCDATA*). 

### DTD externe 

Une DTD est **externe** si elle est localisée dans un autre fichier (en 
*.dtd*).  
On peut l'adresser par une URL 
```<!DOCTYPE root-element SYSTEM "url.dtd">```.  
On peut aussi l'adresser par **FPI** (*Formal Public Identifier*) de la forme  
```type//owner//desc//lang``` : ```-//W3C//DTD XHTML 1.0 Strict//EN```.  
Le *type* est "+" ou "-" selon que le propriétaire respecte la norme ISO 9070 
ou pas.  
On fait l'inclusion : ```<!DOCTYPE root-element PUBLIC "fpi" "url">```. Les 
*FPI* établissent une correspondance via un catalogue. Donc quand on change une 
DTD de place, on ne change que dans le catalogue, pas dans tous les documents. 
L'URL est utilisée quand le FPI ne permet pas de retrouver la DTD.  
  
*Note : on peut convertir un FPI en URI en remplaçant "//" par ":" et " " par 
"+"  
-> urn:publicid:-:W3C:DTD+XHTML+1.0+Strict:EN .* 

### DTD mixte 

On peut mélanger les déclarations internes et externes avec une des deux formes 
suivantes : 
```
<!DOCTYPE root-element SYSTEM "url" [ declarations ]> 
<!DOCTYPE root-element PUBLIC "fpi" "url" [ declarations ]> 
``` 
On ne peut pas déclarer plusieurs fois le même élément. On peut par contre 
déclarer plusieurs fois un même attribut : la 1<sup>ère</sup> déclaration fait 
foi. 

## Contenu d'une DTD 

Une DTD est formée de déclaration d'**éléments**, d'**attributs** et 
d'**entités**. Chaque déclaration est comprise dans  
```<!TYPE ... >```. 

## Entités

### Déclaration et référence 

Déclaration :  
```
<!ENTITY name "fragment_de_document"> : interne 
<!ENTITY name SYSTEM "url"> : externe 
<!ENTITY name PUBLIC "fpi" "url"> : externe 
```
Référence d'une entité *name* :  
```<tag meta="attribute: &name;">Content: &name;</tag>```  
Quand le document est traité, les références sont remplacées par le fragment 
de document correspondant.  
**Attention** : seules les entités internes peuvent être utilisées comme 
valeur d'attribut. Les entités internes ou externes peuvent être utilisées 
comme contenu d'élément. 

### Entités internes

```
<!ENTITY aka   "also known as"> 
<!ENTITY euro  "&#x20AC;"> 
<!ENTITY rceil '<phrase condition="html">&#x2309;</phrase> 
                <phrase condition="fo" role="symbolfont">&#xF8F9;</phrase>'> 
```
On peut définir des entités qui utilisent d'autres entités, pourvu qu'elles 
soit aussi définies (peu importe l'ordre). Les définitions récursives ne sont 
pas possibles. 

### Entités externes 

```
<!DOCTYPE book [ 
  <!-- Entités externes --> 
  <!ENTITY chapter1 SYSTEM "chapter1.xml"> 
  <!ENTITY chapter2 SYSTEM "chapter2.xml"> 
]> 
<book> 
  <!-- Inclusion du fichier chapter1.xml --> 
  &chapter1; 
  <!-- Inclusion du fichier chapter2.xml --> 
  &chapter2; 
</book> 
```

### Entités paramètres

Les **entités paramètres** peuvent être utilisées uniquement à l'intérieur de 
la DTD.  
On les déclare avec ```<!ENTITY % name ... >``` et on les utilise avec 
```%name;``` .  
Le but est d'avoir de la **modularité** : 
```
<!-- Déclaration de deux entités paramètres -->
<!ENTITY % idatt   "id ID #REQUIRED">
<!ENTITY % langatt "xml:lang NMTOKEN 'fr'">

<!-- Utilisation des deux entités paramètres -->
<!ATTLIST chapter %idatt; %langatt;>
<!ATTLIST section %langatt;>
```

## Elements 

Au coeur des DTD, les déclarations d'éléments définissent un document 
*valide*. 

### Contenu pur

Le contenu d'un élément est **pur** si il ne contient que d'autres éléments 
comme enfants : pas de texte. On définit quels peuvent être les enfants, et 
leur ordre d'apparition.  
On les déclare avec ```<!ELEMENT name regexp>``` où *regexp* décrit les suites 
d'enfants valides (avec les opérateurs *, | ? * +*). Pour simplifier la 
validation, il vaut mieux faire des contenus déterministes.  
Exemples :  
```
<!ELEMENT elem (elem1, (elem2 | elem4), elem3)> 
<!ELEMENT elem ((elem1* | (elem2, elem3)), elem4)+> 
``` 

**Pour indenter correctement** : l'en-tête doit avoir ```standalone="no"``` ou 
la DTD doit être interne. Ceci autorise les caractères d'espacement entre les 
fils d'un élément pur. 

### Contenu textuel

Pour spécifier qu'un élément ne peut contenir que du texte : 
```<!ELEMENT name (#PCDATA)>```. 

### Contenu mixte 

Pour mixer le texte et des éléments fils : 
```<!ELEMENT element (#PCDATA | element1 | ... | elementN)*>``` (*#PCDATA* 
doit apparaitre en 1<sup>er</sup>).

### Contenu vide 

```<!ELEMENT element EMPTY>```  
L'élément ne peut avoir que des attributs. Les éléments *vides* font souvent 
le lien entre éléments. 

### Contenu libre 

Si on ne veut aucune contrainte sur un élément (hormis qu'il soit *bien 
formé*) : ```<!ELEMENT element ANY>```.  
C'est raisonnable en cours de mise au point, pas en production : trop laxiste.

## Attributs 

On peut déclarer d'un coup plusieurs attributs pour un élément (indentation 
facultative) :  
```
<!ATTLIST element attribut1 type1 default1 
                  attribut2 type2 default2 
                  ... 
                  attributN typeN defaultN> 
```

### Types des attributs 

- CDATA : le plus général, n'impose aucune contrainte
- (value-1 | ... | value-N) : l'attribut doit avoir comme valeur un des 
*jetons* listés (sans guillemets ni apostrophes)
- NMTOKEN : la valeur est un *jeton*
- NMTOKENS : liste de jetons séparés par des espaces
- ID : la valeur est un *nom XML* unique dans le document
- IDREF : référence à un élément identifié par la valeur de son attribut ID
- IDREFS : liste de références séparés par des espaces
- NOTATION : une notation (non détaillé)
- ENTITY : entité externe non XML (non détaillé)
- ENTITIES : liste d'entités externes non XML (non détaillé)

Les types les plus utilisés sont CDATA, ID et IDREF. 

#### ID, IDREF, IDREFS 

Les attributs ID doivent être uniques, et un élément ne doit avoir d'un seul 
attribut ID. Ils sont utilisés par **XPath.id()**.  
IDREF est un nom XML qui doit aussi être la valeur ID d'un autre élément. 
IDREFS prend une suite d'IDREF.  
```
<?xml version="1.0" encoding="iso-8859-1" standalone="no"?>
<!DOCTYPE book [
  <!ELEMENT book    (section)*>
  <!ELEMENT section (#PCDATA | ref | refs)*>
  <!ATTLIST section id ID #IMPLIED>
  <!ELEMENT ref     EMPTY>
  <!ATTLIST ref     idref  IDREF  #REQUIRED>
  <!ELEMENT refs    EMPTY>
  <!ATTLIST refs    idrefs IDREFS #REQUIRED>
]>
<book>
  <section id="sec0">Une référence <ref idref="sec1"/></section>
  <section id="sec1">Des références <refs idrefs="sec0 sec2"/></section>
  <section id="sec2">Section sans référence</section>
  <section id="sec3">Une auto-référence <refs idrefs="sec3"/></section>
</book>
```

### Valeurs par défaut 

Les valeurs par défaut peuvent être : 

- "valeur" ou 'valeur' : une chaine
- #IMPLIED : attribut optionnel, n'a donc pas de valeur par défaut (sans 
valeur si absent)
- #REQUIRED : obligatoire et donc sans valeur par défaut
- #FIXED "value" ou #FIXED 'value' : valeur fixée. Si absent, il vaut 
*"value"*. Si présent, il doit valoir *"value"* pour que le document soit 
valide.

