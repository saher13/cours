# XML - Cours 5 : Schémas

```
<?xml version="1.0" encoding="iso-8859-1"?>
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"> (1)
  <xsd:annotation> (2)
    <xsd:documentation xml:lang="fr">
      Schéma XML pour bibliography.xml
    </xsd:documentation>
  </xsd:annotation>
  <xsd:element name="bibliography" type="Bibliography"/> (3)

  <xsd:complexType name="Bibliography"> (4)
    <xsd:sequence>
      <xsd:element name="book" minOccurs="1" maxOccurs="unbounded"> (5)
        <xsd:complexType>
          <xsd:sequence>
            <xsd:element name="title"     type="xsd:string"/>
            <xsd:element name="author"    type="xsd:string"/>
            <xsd:element name="year"      type="xsd:string"/>
            <xsd:element name="publisher" type="xsd:string"/>
            <xsd:element name="isbn"      type="xsd:string"/>
            <xsd:element name="url"       type="xsd:string" minOccurs="0"/>
          </xsd:sequence>
          <xsd:attribute name="key"  type="xsd:NMTOKEN" use="required"/> (6)
          <xsd:attribute name="lang" type="xsd:NMTOKEN" use="required"/> (7)
        </xsd:complexType>
      </xsd:element>
    </xsd:sequence>
  </xsd:complexType>
</xsd:schema>
```

- (1) : élément racine avec la déclaration de l'espace de noms associé à
*xsd*
- (2) : documentation du schéma
- (3) : déclaration de l'élément *bibliography* de type *Bibliography*
- (4) : début de la définition du type *Bibliography*
- (5) : Déclaration de l'élément *book* dans le contenu du type *Bibliography*
- (6)(7) : Déclaration des attributs *key* et *lang* (de type *xsd:NMTOKEN*) de
l'élément *book*

## Structure globale d'un schéma

Un **schéma XML** se compose de déclarations d'éléments et d'attributs, et de
définitions de types.  
Chaque élément est déclaré avec un type (prédéfini ou un nouveau type, obtenu
par *construction* ou *dérivation*).  
Un schéma est associé à l'espace de nom ```xsd``` ou ```xs```. Tout schéma est
inclus dans l'élément ```xsd:schema```.  
Les éléments, attributs, et type peuvent être *locaux* ou *globaux* (enfant
direct de *xsd:schema*). Les types locaux n'ont pas de noms. Tous les éléments
et attributs (locaux ou globaux) ont un nom.

### Attributs de xsd:schema

- targetNamespace : l'espace de noms cible. Pas d'espace de noms si absent.
- elementFormDefault, attributeFormDefault : valeur par défaut de l'attribut
  ```form``` pour les éléments et les attributs. Valeurs : ```qualified``` ou
  ```unqualified``` (par défaut)
- blockDefault : valeur par défaut des attributs ```block```, peut prendre
  la valeur ```#all``` ou une liste parmi
  ```extension, restriciton, substitution```
- finalDefault : valeur par défaut des attributs ```final```. Les valeurs
  possibles sont ```#all``` ou une liste parmi
  ```extension, restriction, list, union```

### Référence à un schéma

Un document XML peut être validé par schéma. On le spécifie dans un des
attributs ```schemaLocation``` ou ```noNamespaceSchemaLocation``` dans
l'élément racine du document à valider. La valeur est de la forme :  
```schemaLocation="ns1 sch1 ns2 sch2 ... nsN schN"```  
où *ns<sub>i</sub>* est l'espace de noms cible du schéma *sch<sub>i</sub>*.  
Si le document n'utilise pas de namespace, on utilise l'autre, avec 1 seule
URL : le chemin vers le schéma.

## Déclarations d'éléments

Un élément apparaissant dans le document doit être déclaré dans le schéma.
Cette déclaration se fait en 2 parties : d'une part les contenus possibles,
d'autre part les attributs autorisés et obligatoires.

### Type nommé

```<xsd:element name="element" type="type" />```  
Le nom du type doit être un nom qualifé.  
Quand le type est simple, on peut donner une valeur par défaut :
```
<xsd:element name="title" type="xsd:string" default="Titre défault" />
<xsd:element name="title" type="tns:Title" fixed="Titre fixe "/>
```

### Type anonyme

On peut aussi décrire explicitement le type à la déclaration de l'élément :
c'est alors le contenu. Le type est alors *local*.
```
<xsd:element name="element">
  <xsd:simpleType>
  ...
  </xsd:simpleType>
</xsd:element>
```
```
<xsd:element name="element">
  <xsd:complexType>
  ...
  </xsd:complexType>
</xsd:element>
```

### Référence à un élément

Un élément global est utilisable dans les définitions de type :  
```
<xsd:element name="title" type="Title"/>
...
<xsd:complexType ... >
  ...
  <!-- Utilisation de l'élément title -->
  <xsd:element ref="title"/>
  ...
</xsd:complexType>
```
On ne peut pas avoir à la fois ```name``` et ```ref```, mais un des deux doit
être présent.  
Deux éléments non globaux peuvent avoir le même nom avec des types différents.

## Définition de types

Les schémas distinguent les types **simples** et les types **complexes**.  
Tous deux sont dérivés de ```xsd:anyType``` (qui est aussi le type par défaut
quand aucun n'est spécifié).  
Les types simples décrivent du texte, et sont utilisables à la fois en élément
et en attributs. On les obtient en dérivant un type *prédéfini*.
Les types *complexes* décrivent les contenus pouvant être constitué d'éléments.
Eux seuls peuvent définir des attributs.  
On peut définir une **hiérarchie de types**, obtenus par *extension* ou
*restriction* de types déjà existants.

### Types prédéfinis

Tous appartiennent à l'espace de nom ```xsd```. Le préfixe est ici omis.

- numériques : *boolean, byte, unsignedByte, short, unsignedShort, int,
unsignedInt, long, unsignedLong, integer, positiveInteger* (strict)*,
negativeInteger *(strict)*,
nonPositiveInteger, nonNegativeInteger, float, double, decimal.*
- chaînes et noms : *string, normalizedString, token, Name* (nom XML)*,
QName* (nom qualifié)*, NCName *(non-qualifié, sans ':')*, language, anyURI,
base64Binary, hexBinary*
- date et heure : *time *(hh:mm:ss)*, date *(YYYY-MM-DD)*, dateTime
*(YYYY-MM-DDThh:mm:ss)*, duration, dayTimeDuration, yearMonthDuration, gYear
*(YYYY)*, gMonth, gMonthDay, gDay*.
- hérités des DTD : *ID, IDREF, IDREFS, NMTOKEN, NMTOKENS, ENTITY, ENTITIES,
NOTATION*

### Types simples

Utilisables pour des éléments ou des attributs.  
```
<xsd:simpleType name="Byte">
  <xsd:restriction base="xsd:nonNegativeInteger">
    <xsd:maxInclusive value="255"/>
  </xsd:restriction>
</xsd:simpleType>
```
Un type simple peut avoir un attribut *name* si il est global.

### Types complexes

Utilisables uniquement pour des éléments.
```
<xsd:complexType ...>
  <!-- Construction du type avec xsd:sequence, xsd:choice ou xsd:all -->
  ...
</xsd:complexType>
```
Si le type est obtenu par extension ou restriction d'un autre type, l'élément
```xsd:complexType``` doit contenir un élément ```xsd:simpleContent``` ou
```xsd:complexContent``` qui précise si le contenu est purement textuel ou non.
```
<!-- Type dérivé à contenu textuel -->
<xsd:complexType ...>
  <xsd:simpleContent>
    <!-- Extension ou restriction -->
    ...
  </xsd:simpleContent>
</xsd:complexType>
<!-- Type dérivé à contenu pur ou mixte -->
<xsd:complexType ...>
  <xsd:complexContent>
    <!-- Extension ou restriction -->
    ...
  </xsd:complexContent>
</xsd:complexType>
```

### Contenu mixte

Les types complexes ont un attribut ```mixed```, qui quand il vaut *true*,
autorise la construction d'un type avec contenu mixte.

## Construire des types

### Element vide

Un type complexe qui ne déclare que des attributs, doit avoir un contenu vide.
```
<xsd:element name="link" type="Link"/>
<xsd:complexType name="Link">
  <xsd:attribute name="ref" type="xsd:IDREF" use="required"/>
</xsd:complexType>
```

### Séquence

```xsd:sequence``` définit un nouveau type formé d'une suite des éléments
énumérés, ils doivent tous être présent dans l'ordre. Ils peuvent être
explicites, référencés (avec ```ref```), ou construits récursivement.
```
<xsd:complexType>
    <xsd:sequence>
      <xsd:element name="title"     type="xsd:string"/>
      <xsd:element name="author"    type="xsd:string"/>
      <xsd:element name="year"      type="xsd:string"/>
      <xsd:element name="publisher" type="xsd:string"/>
    </xsd:sequence>
  </xsd:complexType>
```

### Choix

```xsd:choice``` définit un type formé d'**un** des éléments énumérés.  
```
<xsd:complexType>
    <xsd:choice>
      <xsd:element name="author" type="xsd:string"/>
      <xsd:element name="authors">
        <xsd:complexType>
          <xsd:sequence>
            <xsd:element name="author" type="xsd:string"
                         minOccurs="2" maxOccurs="unbounded"/>
          </xsd:sequence>
        </xsd:complexType>
      </xsd:element>
    </xsd:choice>
  </xsd:complexType>
```

### Ensemble

```xsd:all``` définit un type dont chacun des éléments énumérés doit apparaître
une fois, dans un ordre quelconque.
```
<xsd:complexType>
    <xsd:all>
      <xsd:element name="title"     type="xsd:string"/>
      <xsd:element name="author"    type="xsd:string"/>
      <xsd:element name="year"      type="xsd:string"/>
      <xsd:element name="publisher" type="xsd:string"/>
    </xsd:all>
  </xsd:complexType>
```
Cet élément ne peut pas être imbriqué avec des séquences, des choix, ou un
autre ensemble. De plus, c'est toujours un enfant de ```xsd:complexType```
ou ```xsd:complexContent```.  
Il possède les attributs ```minOccurs``` (qui vaut "0" ou "1"), et
```maxOccurs``` (qui doit valoir "1", et vaut "1" par défaut). Ces valeurs
s'appliquent à tous les enfants.

### Union

```xsd:union``` définit un type simple dont les valeurs sont celles des types
listés dans son attribut ```memberTypes```.
```
<xsd:simpleType name="IntegerOrUnbounded">
  <xsd:union memberTypes="xsd:nonNegativeInteger Unbounded"/>
</xsd:simpleType>
<xsd:simpleType name="Unbounded">
  <xsd:restriction base="xsd:string">
    <xsd:enumeration value="unbounded"/>
  </xsd:restriction>
</xsd:simpleType>
```
ou
```
<xsd:union memberTypes="xsd:nonNegativeInteger">
    <xsd:simpleType>
      <xsd:restriction base="xsd:string">
        <xsd:enumeration value="unbounded"/>
      </xsd:restriction>
    </xsd:simpleType>
  </xsd:union>
```

### Liste

```xsd:list``` définit un type simple dont les valeurs des listes du type
donné par l'attribut ```itemType```.  
```
<!-- Type pour les listes d'entiers -->
<xsd:simpleType name="IntList">
  <xsd:list itemType="xsd:integer"/>
</xsd:simpleType>
<!-- Type pour les listes de 5 entiers -->
<xsd:simpleType name="IntList5">
  <xsd:restriction base="IntList">
    <xsd:length value="5"/>
  </xsd:restriction>
</xsd:simpleType>
```

### Répétitions

```minOccurs``` et ```maxOccurs``` précisent le nombre d'occurences d'un
élément ou d'un groupe. Il prennent un entier en valeur (la borne supérieure
peut en plus prendre *unbounded*).  
Par défaut, chaque attribut vaut *"1"*.

### Joker

```xsd:any``` permet d'introduire des éléments externes au schéma (donc non
définis). On peut spécifier les bornes d'occurences.  
La validation est contrôlée avec l'attribut ```processContent``` qui peut être
```strict``` (par défaut, doivent être validés par un autre schéma),
```skip``` (non validés), ou ```lax``` (on tente la validation, mais on
autorise l'échec).  
On peut vouloir restreindre ces éléments à un espace de nom, dans ce cas on
spécifie l'attribut ```namespace```.

### Validation

Le contenu des éléments purs et mixtes doit être **déterministe**, c'est à
dire qu'en voyant une balise ouvrante, dans le contenu, on doit pouvoir
déterminer d'où elle provient dans la définition.  
Contre-exemple (*item1* peut provenir de deux chemins) :  
```
<xsd:element name="item">
    <xsd:complexType>
      <xsd:choice>
        <xsd:sequence>
          <xsd:element name="item1" type="xsd:string"/>
          <xsd:element name="item2" type="xsd:string"/>
        </xsd:sequence>
        <xsd:sequence>
          <xsd:element name="item1" type="xsd:string"/>
          <xsd:element name="item3" type="xsd:string"/>
        </xsd:sequence>
      </xsd:choice>
    </xsd:complexType>
  </xsd:element>
```

## Déclarer des attributs

La déclaration d'un **attribut** est comme celle d'un élément sauf qu'on
utilise ```xsd:attribute```. Le type doit forcément être un type simple (car
un attribut ne peut contenir que du texte).  
```<xsd:attribute name="format" type="xsd:string"/>```  
On déclare les attributs dans la définition d'un type (**complexe
uniquement**). On peut aussi faire des attributs *globaux* en les plaçant
comme enfants de ```xsd:schema```. Ils peuvent alors être ajoutés à différents
types complexes avec un attribut ```ref``` qui doit avoir comme valeur le nom
de l'attribut global à ajouter.  

L'attribut ```use``` change les contraintes de présence : ```optional``` (par
défaut), ```required, prohibited```.  
La valeur par défaut est donnée par les attributs ```default``` ou ```fixed```.

```xsd:anyAttribute``` fonctionne de manière similaire à ```xsd:any```.

## Extension de types

L'**extension** donne toujours un type complexe. Elle est introduite par
l'élément ```xsd:extension``` qui possède un attribut ```base``` dont la
valeur est le nom du type de base.  

Quand on étend un type simple ou un type complexe à contenu simple, on ne
peut que rajouter des attributs :  
```
xsd:complexType name="Price">
  <xsd:simpleContent>
    <xsd:extension base="xsd:decimal">
      <!-- Attribut ajouté -->
      <xsd:attribute name="currency" type="xsd:string"/>
    </xsd:extension>
  </xsd:simpleContent>
</xsd:complexType>
<xsd:element name="price" type="Price"/>
```
En étendant un type complexe à contenu complexe, on peut aussi ajouter du
contenu :  
```
 <!-- Type de base -->
  <xsd:complexType name="Name">
    ...
  </xsd:complexType>
  <!-- Extension du type de base -->
  <xsd:complexType name="Fullname">
    <xsd:complexContent>
      <xsd:extension base="Name">
        <xsd:sequence> <!-- ajouté après le contenu déjà présent -->
          <xsd:element name="title" type="xsd:string"/>
        </xsd:sequence>
        <xsd:attribute name="id" type="xsd:ID"/>
      </xsd:extension>
    </xsd:complexContent>
  </xsd:complexType>
```

## Restriction 

La **restriction** s'introduit avec l'élément ```xsd:restriction```, qui possède un attribut ```base``` qui doit prendre le nom du type de base. On utilise ensuite des *facettes*.  

### Restriction par intervalle 

```
<xsd:element name="year"> 
  <xsd:simpleType>
    <xsd:restriction base="xsd:integer">
      <xsd:minInclusive value="1970"/>
      <xsd:maxInclusive value="2050"/>
    </xsd:restriction>
  </xsd:simpleType>
</xsd:element>
```

### Restriction par énumération 

```
<xsd:element name="language" type="Language"/> 
<xsd:simpleType name="Language">
  <xsd:restriction base="xsd:language">
    <xsd:enumeration value="de"/>
    <xsd:enumeration value="en"/>
    <xsd:enumeration value="fr"/>
  </xsd:restriction>
</xsd:simpleType>
```

### Restriction par motif 

```
<xsd:simpleType name="Identifier">
  <xsd:restriction base="xsd:string">
    <xsd:pattern value="[:_A-Za-z][-.:_0-9A-Za-z]*"/>
  </xsd:restriction>
</xsd:simpleType>
```
Pour avoir une expression qui accepte éventuellement un fragment du contenu, il suffit d'ajouter ```.*``` au début et à la fin de celle-ci. Le contenu *abc123xyz* est, par exemple, conforme à l'expression ```.*\d{3}.*```.

### Autres facettes 

- longueur : ```xsd:length, xsd:minLength``` et ```xsd:maxLength```
- digits : ```xsd:fractionDigits``` et ```xsd:totalDigits```
- espacement : ```xsd:whiteSpace``` modifie le traitement des espaces, selon sa valeur (```preserve, replace, collapse```)
