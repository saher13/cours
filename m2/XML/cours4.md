# XML - cours 4 

## Identification d'un espace de noms 

Un **espace de noms** est identifié par *l'URI de l'espace de noms*, très 
souvent une URL. Cet URI garantit que l'espace de noms est unique. Dans la 
pratique, l'URL permet d'accéder à un document qui décrit l'espace de noms. 

## Déclaration d'un espace de noms 

Les espaces de noms sont déclarés par un pseudo-attribut de la forme 
```xmlns:prefix``` où *prefix* est un nom XML ne contenant pas ':' qui sera le 
nom qualifié de l'espace de noms.  

## Portée 

La portée d'un espace de noms est limitée à l'élément dans lequel il est déclaré, 
balises comprises. 
```
<html:html xlmns:html="http://www.w3.org/1999/xhtml">
  <html:head>
    <html:title>Titre</html:title>
  </html:head>
  ...
</html:html>
```

## Espace de noms par défaut

Quand on ne déclare pas d'espaces de noms, les noms utilisés n'appartiennent à 
aucun espace. En fait la valeur de l'attribut **espace de nom** est nul.  
Si on déclare un espace de noms par défaut, ils appartiennent à celui-ci.  
On déclare l'espace de noms par défaut comme un espace de nom normal, mais 
avec un préfixe vide. 
```
<html xmlns="http://www.w3.org/1999/xhtml">
  <head>
  ...
  </head>
  ...
</html>
```
Comme la portée d'un espace de noms est locale, on peut changer l'espace de 
noms par défaut en cours de route. L'ancien espace de noms par défaut revient à 
la sortie de l'élément.  
  
Est-ce nécessaire ? 

- Les choses simples n'ont pas besoin d'espaces de noms.  
- Avec les DTD, c'est à éviter (voir section dédiée).  
- En utilisant des schémas, on est obligé de les utiliser. On cherche alors à 
utiliser au maximum les espaces de noms par défaut.  

## Attributs 

Un attribut peut faire partie d'un espace de noms si il est préfixé. Sans préfixe, 
ils n'ont aucun espace de nom (même pas celui par défaut).  
```
<book version="5.0"  // version n'appartient à aucun espace de noms
      xml:id="course.xml" // id appartient à l'espace de noms xml
      xmlns="http://docbook.org/ns/docbook"> // l'espace par défaut est docbook
```

## Espace de noms XML 

Le préfixe *xml* est toujours lié (implicitement) à l'espace de noms XML, sans 
avoir à être déclaré. Celui-ci est identifié par l'URI 
*http://www.w3.org/XML/1998/namespace*.  
Cet espace contient les 4 attributs particuliers : *xml:lang*, *xml:space*, 
*xml:base*, *xml:id*.

## Espaces de noms et DTD 

Les DTD ignorent les espaces de noms. Mais on peut vouloir valider un document 
avec ses espaces de noms. 
```
<!-- Fichier "valid.dtd" valide avec l'espace de noms "tns" -->
<!ELEMENT tns:list (tns:item)+>
<!ATTLIST tns:list xmlns:tns CDATA #REQUIRED>
<!ELEMENT tns:item (#PCDATA)>
``` 
Toutefois, si on veut changer le nom de l'espace, il faut aussi changer la DTD. 