# XML - Cours 2 : Syntaxe

La syntaxe XML est très simple : quelques règles pour l'entête, et des balises 
pour structurer les données. Contrairement à HTML, les noms des balises sont 
libres, mais chaque balise ouverte doit être fermée. 

## Example 

Entête : ```<?xml version="1.0" encoding="UTF-8"?>```.  
Commentaire : ```<!-- commentaire -->```.  
DTD externe : ```<!DOCTYPE bibliography SYSTEM "bibliography.dtd" >```.  
Balises sans attributs : ```<balise>...</balise>```.  
Balise avec attributs : ```<balise key="a" lang="fr">...</balise>```.  

## Caractères 

Les caractères utilisables sont ceux de la norme Unicode ISO 10646 (Universal 
Character Set : tous des les caractères connus). Ils sont codés sur 32 bits 
avec un *point de code* situé entre 0 et 0x10FFFF (ils utilisent donc au 
maximum 21 bits). On désigne un caractère de code *n* par U+m où *m* est 
l'écriture hexadécimale de *n* (sans mettre le 'x').  
Exemple : € = 8364 = U+20AC.  
  
Le sous-ensemble des caractères Unicode tenant sur 16 bits (entre 0 et 0xFFFF) 
est le BPM (Basic Multilingual Plane). Il couvre largement la majorité des 
langues usuelles et symboles courants.  

### Caractères spéciaux 

- < : U+2C, début de balise
- > : U+2E, fin de balise
- & : U+26, référence aux entités générales
- ' : U+27, délimiteur d'attributs
- " : U+22, délimiteur d'attributs

### Caractères d'espacement 

Chaque caractère d'espacement équivaut à un espace. Une suite d'espace 
équivaut aussi à un espace. 

- U+20, espace
- U+09, tabulation \t
- U+0A, saut de ligne \n
- U+0D, retour chariot \r 

Les différentes combinaisons sont remplacées lors du parsing par un seul U+0A 
avant la transmission à l'application, afin de garantir une indépendance par 
rapport au système d'exploitation. Ces combinaisons sont : 

- U+85 (next line)
- U+0D puis U+0A
- U+0D puis U+85
- U+0D (non suivi de U+01 ou U+85)
- U+2028 (line separator)

On déconseille l'usage de U+85 et U+2028 dans l'enête car ils ne peuvent être 
correctement décodés qu'après déclaration de l'encodage. 

### Jetons et noms XML 

Ce sont des identificateurs, qui peuvent contenir tous les caractères 
alphanumériques : [a-z], [A-Z], [0-9], - (U+2D), . (U+2E), : (U+3A), _ (U+5F).  
Un **jeton** est une suite quelconque de caractères. Un **nom XML** est un 
jeton qui commence par une lettre, : ou _ (donc pas de chiffre, - ou :). A 
priori leur taille n'est pas limitée mais les logiciels peuvent en imposer 
une.  
Le caractère ':' est réservé aux *espaces de noms*, il sépare le préfixe du 
nom local. Si il est présent, c'est un **nom qualifié**, sinon c'est un nom non 
qualifié.  
Les noms commencçant par [xX][mM][lL] sont réservés aux usages internes de XML. 
Ils ne peuvent pas être utilisés librement mais peuvent apparaitre pour des 
utilisations spécifiques prévues par la norme. Les noms commençant par *xml:* 
font partie de l'espace de noms XML.  
La norme XML 1.1 prévoit l'utilisation de tout caractère Unicode dans les 
identificateurs. Il vaut mieux se limiter à ASCII pour des raisons de 
compatibilité. 

### Codage 

Afin de réduire la taille des fichiers, certains caractères peuvent être codés 
sur moins de 32 bits : le caractère ne contient pas directement les points de 
code. On va donc coder les points de code, selon le codage défini dans 
l'entête. Les principaux codages sont : 

- UTF-8 (par défaut), entre 1 et 4 octets. L'ASCII n'est codé que sur 1 octet 
avec 0 comme bit de poids fort. Hors ASCII, il faut au moins 2 octets. 
- UTF-16, codant le BPM, sauf la plage 0xD800 à 0xDFFF. Cela ne pose pas de 
soucis car ces points de codes ne sont pas attribués). 
- US-ASCII, de 0 à 0x7F de l'ASCII. 
- UCS-4 ou UTF-32. Prend le point de code exact. 
- UCS-2, point de code sur 2 octets (BMP). 
- ISO-8859-1 (Latin-1), 1 seul octet. Langues d'Europe de l'ouest. 
- ISO-8859-15 (Latin-9 ou Latin-0). Mise à jour de Latin-1 avec 8 différences. 

Les logiciels manipulant XML doivent obligatoirement savoir gérer UTF-8 et 
UTF-16. Les autres sont facultatifs. On peut de toutes façons insérer 
n'importe quel caractère unicode directement avec son point de code selon 
deux syntaxes : *&#<code décimal>;* ou *&#x<code hexadécimal>;*.  

#### Ordre des octets 

Pour savoir si un document XML utilise le mode petit-boutiste ou gros-boutiste, 
on sait que le document commence par U+FEFF (*espace insécable de largeur 
nulle*). Il a depuis été remplacé par U+2060, donc U+FEFF est maintenant 
utilisé exclusivement comme marque d'ordre (Byte Order Mark, ou BOM).  
Comme il n'existe pas de caractère de point de code 0xFFFE, il n'y a pas 
d'ambiguité. On regarde donc la suite d'octet, sachant que les premières 
lettres du document sont 0x3C, 0x3F et 0x78 (*<?x*).  

- UTF-8 sans BOM : 3C 3F 78
- UTF-8 avec BOM : EF BB BF 3C 3F 78
- UTF-16/UCS-2 en big-endian : FE FF 00 3C 00 3F
- UTF-16/UCS-2 en little-endian : FF FE 3C 00 3F 00
- UTF-32 en big endian : 00 00 FE FF 00 00 00 3C ...
- UTF-32 en little endian : FE FF 00 00 3C 00 00 00 ... 

Note : BOM ne peut pas être mise en ISO-8859-1, et est déconseillée en UTF-8 
car inutle. 

### Collations 

Les collations définissent une collection de règles d'équivalences entre 
caractères ou suites de caractères, et l'ordre lexicographique.  
Exemples : fusionner 'o' et 'e' dans *oe*uf, on définir 'é' > 'z' (donc 
que "zèbre" < "étalon").  

### Normalisation 

Un caractère peut avoir plusieurs points de code. Unicode introduit des 
normalisations qui transforme les différents codages en un codage canonique. 
La normalisation standard est celle de C. La normalisation d'une chaîne peut 
être obtenue avec la fonction XPath *normalize-unicode()*. 


## URI, URL, URN 

XML fait un usage massif des **URI** (*Uniform Resource Identifier*), notamment 
des URL (*Uniform Resource Locator*) pour référencer des documents externes 
(comme les DTD). XML distingue aussi les URI et les URN (*Uniform Resource 
Name*).  
La notion la plus générale est celle d'URI, car elle contient les URL et les 
URN (sachant que certains URI peuvent être à la fois URN et URL).  
Un URI est un identifiant désignant sans ambiguité une ressource, il doit donc 
être universellement unique. L'URL est composée d'un protocole qui permet 
d'accéder au document, là où l'URN donne uniquement le nom, indépendamment de 
la façon d'y accéder.  
La syntaxe générale est **scheme:ident** où *scheme* est un schéma d'URI et 
*ident* l'identifiant obéissant à la syntaxe de *scheme*. Les schémas 
définissent des sous-espaces d'URI. Pour une URL, le schéma est un protocole 
d'accès (http, sip...). Le schéma pour les URN doit être *urn*, et est en 
général suivi d'un espace de nom.  
Exemples : 

- http://www.omega-one.org/~carton/
- sftp://carton@omega-one.org
- tel:+33-1-57-27-92-54
- sip:0957279254@freephonie.net
- file://home/carton/Enseignement/XML/Cours/XSLT
- urn:oasis:names:tc:docbook:dtd:xml:docbook:5.1
- urn:publicid:-:W3C:DTD+HTML+4.0:EN

### Résolution d'URI 

Un URI *de base*, généralement une URL, est souvent attaché à un document (ou 
fragment de document). Il permet de résoudre les URL contenues dans le 
fragment de document. La résolution consiste ) combiner l'URI de base avec ces 
URL pour obtenir des URL absolues, qui désignent des documents externes.  
Chaque URL se décompose en 3 parties : 

- Protocole d'accès : nom du protocole suivi de ':'. Les principaux sont 
*http, https, file* et *ftp* 
- Adresse internet : commence par "//", absente pour le protocole *file* 
- Chemin d'accès : dans l'arborescence. Contient le nom du répertoire, et le 
nom du fichier (après le dernier '/'). 
  
La combinaison d'un URL de base (*base) avec une autre URL (*url*) dépend de 
la forme de *url* : 

- Si *url* est déjà une URL complète, le résultat est *url*, sans tenir compte 
de *base* 
- Si *url* est un chemin absolu commençant par '/', on change la partie 
"chemin" de *base* par *url*. *url* est ajoutée après l'adresse internet. 
- Si *url* est un chemin relatif, on obtient *base* + *url*. 

Exemples avec *base*=http://www.toto.net/A/foo.html : 

- url="" : http://www.toto.net/A/foo.html
- url="B/bar.html" (relatif) : http://www.toto.net/A/B/bar.html 
- url="/B/bar.html" (absolu) : http://www.toto.net/B/bar.html
- url="http://www.titi.net/abc.html" (complète) : http://www.titi.net/abc.html


## Syntaxe et structure 

Pour qu'un document XML soit correct, il doit d'abord être **bien formé** 
(contrainte syntaxique), puis être **valide** (contrainte structurelle).  
Un document valide doit respecter un *modèle de document*, qui définit la 
grammaire. Pour écrire un modèle, on peut utiliser les *DTD (Document Type 
Description)*, simples mais limitées, ou les *schémas XML* (plus puissants).  

## Prologue 

Le prologue contient 2 déclarations facultatives (mais conseillées) : l'entête 
XML, qui précuse la version et le codage du fichier, et la déclaration du 
type de document (omise lors de l'utilisation de schémas). Peuvent suivre, des 
commentaires et des instructions de traitement.  

### Entête XML 

Elle doit se trouver au tout début du document et suivre la forme générale 
suivante :  
```<?xml version="..." encoding="..." standalone="..."?>```

- version : 1.0 ou 1.1 
- encodage : en majuscules, voir section dédiée. UTF-8 par défaut 
- standalone : "yes" si le fichier est autonome (pas de déclarations externes 
qui affectent le document). "no" par défaut. 

### Déclaration de type de document 

Définit la structure du document : ```<!DOCTYPE ... >```. 


## Corps du document 

Le corps est composé d'**éléments** organisés de façon hiérarchique. Chaque 
élément peut contenir du texte simple ou d'autres éléments.  
Il existe un élément qui contient l'intégralité du document : la **racine**. 

### Elements 

```
<name> ...contenu... </name>
```  
Les noms des éléments sont des *noms XML* quelconques. Le *contenu* peut être 
composé de texte, d'autres éléments, de commentaires et d'instructions de 
traitement.  
Dans la balise ouvrante, le nom doit immédiatement suivre '<' (U+3C), mais il 
peut y avoir des espaces entre la fin du nom et '>' (U+3E). La balise fermante 
ne doit pas contenir d'espaces.  
On peut aussi faire une contraction pour une balise de contenu vide : 
```<vide/>```. On privilégie cette notation lorsque l'élément est déclaré vide 
par une DTD.  
Les éléments ne peuvent pas s'imbriquer de manière croisée : AABB ou ABBA sont 
corrects, pas ABAB. 

### Sections littérales 

On ne peut pas écrire directement dans le contenu les caractères spéciaux '>', 
'>' et '&', il faut passer par les *entités prédéfinies*. Mais cela peut 
être long si il y en a beaucoup. Il existe donc les **sections littérales**, ou 
**sections CDATA**, qui inclue des caractères recopiés à l'identique.  
```
<![CDATA[...contenu...]]>
```  
Une section CDATA ne peut pas contenir la chaine "]]>" car elle détecte 
la fin de section. Il est donc impossible d'imbriquer ces sections.  

### Attributs 

Les balises ouvrantes ou contractées peuvent contenir des **attributs** 
associés à des valeurs. L'indentation entre attributs est libre.  
```<tag attribute1="value1" attribute2='value2'>...</tag>```.  
Le nom des attributs doit être un *nom XML*. La valeur peut contenir des 
caractères spéciaux si introduits par les entités prédéfinies. 
L'ordre des attributs n'a pas d'importance, mais ils doivent avoir des noms 
différents.  
On utilise les attributs pour les méta-données plutôt que pour les données 
elles-mêmes, qui doivent être placées dans le contenu.  
```<date format="ISO-8601">2009-01-08</date>``` 

### Attributs particuliers 

#### xml:lang 

Utilisé pour décrire la langue du contenu. Elle doit se transmettre aux 
enfants dans l'application. Sa valeur est un code de langue sur 2 ou 3 lettres 
défini dans la norme *ISO 639* (*fr, en, es...*). Il peut être séparé par un 
tiret '-' d'un code de pays sur 2 lettres de la norme *ISO 3166*.  
```<p xml:lang="fr">Bonjour</p>
<p xml:lang="en-GB">Hello</p>```  
L'attribut *xml:lang* est du type *xsd:language*. 

#### xml:space 

Définit le traitement des caractères d'espacement. La valeur peut être 
**default** ou **preserve**. Cet attribut intervient notamment dans le 
traitement des espaces par XSLT. Il doit s'appliquer à tout le contenu de 
l'élément. 

#### xml:base 

C'est l'*URI de base* dont on a parlé plus tôt. Elle est généralement plutôt 
spécifiée par l'application qui traite le document. Il s'applique à tout 
le contenu, et est généralement un attribut de la racine. 

#### xml:id

De type *xsd:id*, il associe un identificateur à tout élément, indépendamment 
d'une DTD ou d'un schéma. 

### Commentaires 

Il peuvent contenir les caractères spéciaux, mais pas "--", et ne peuvent donc 
pas être imbriqués. Ils peuvent apparaitre partout dans le prologue, dans la 
DTD, ou le contenu d'un élément (pas dans les balises). 

### Instructions de traitement 

Elles sont destinées aux applications qui traitent le document XML. Elles sont 
délimitées par **"<?"** et **"?>"**, où le nom XML de l'instruction suit 
immédiatement "<?", et est suivi de son contenu.  
```
<?dbhtml filename="index.html"?>
<?xml-stylesheet href="list.xsl" type="text/xsl" title="En liste"?>
```


## XInclude 

Permet de scinder un document en plusieurs parties bien formées. 
```
<book version="5.0"  
      xmlns="http://docbook.org/ns/docbook"
      xmlns:xi="http://www.w3.org/2001/XInclude">
  ...
  <!-- Inclusion des différents chapitres -->
  <xi:include href="introduction.xml"   parse="xml"/>
  <xi:include href="Syntax/chapter.xml" parse="xml"/>
  ...
</book>
```