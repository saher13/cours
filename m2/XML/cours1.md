# XML - Cours 1 - Présentation

XML = eXtended Markup Language. C'est un format de documents orienté texte.  
C'est un standard de stockage ou de transmission de données.  
Il est simple, flexible, et extensible.  

Le XML dérive de SGML et HTML, et reprend leur organisation en balises.  

## Intérêt 

XML possède plusieurs qualités :  
 
- séparation stricte entre contenu et présentation 
- simplicité, universalité, extensibilité 
- format texte avec gestion des caractères spéciaux, utilisable avec un éditeur
- structuration forte 
- modèles de documents (pour une vérification automatique) 
- format libre 

## Langages apparentés 

XLink / Xpointer (liens entre documents), XPath (langage de séléction pour 
XSLT), XQuery (langage de requête, d'extraction), Schémas XML (modèles), 
XSLT (transformation de documents). 

## Dialectes 

De nombreux dialectes ont été définis à partir de XML. Ils partagent la 
syntaxe de base, donc tous les outils utilisables pour XML sont aussi 
utilisables pour ces dialectes. On retrouve notamment RSS, XUL, SVG, SMIL, 
MathML, WSDL, OpenStreetMap, SAML, UBL, OpenDocument (LibreOffice), DocBook... 

## DocBook 

C'est un format d'écriture de documents techniques, adapté spécialement à la 
documentation de logiciels (KDE).  
Le texte est réparti entre plsuieurs fichiers XML, qui sont convertis avec 
XSLT en LaTeX pour avoir une mise en page, puis produits en PDF.  