# MacOSX - Cours 2 : Objective-C

## Syntaxe et terminologie 

Objective-C (abbrégé en *ObjC*) utilise une terminologie **pure**. Son 
mécanisme central est l'**expression** d'*envoi de message* : 
```[unePile push:uneValeur];```  
Ici le **receveur** est *unePile*, et reçoit le **message** *push:uneValeur* 
(le **sélécteur** est *push:*). La **méthode** est le code qui sera 
séléctionné.  
Les **accesseurs** ont une syntaxe spéciale : 

- getter : ```int x = [image sizeX];```, qu'on peut tout de même écrire 
```image.sizeX``` (y compris si *image* est un pointeur). 
- setter : ```[image setSizeX:1200];```, qu'on peut tout de même écrire 
```image.sizeX = 1200;``` 

Il y a deux typages, l'un *dynamique* (```id uneRef;```), l'autre *statique* 
(```Stack * unePile;```). Les types sont vérifiés à la compilation, mais la 
liaison est dynamique.  
**id** permet d'introduire un objet sans préciser 
son type. C'est aussi le type par défaut renvoyé par toute méthode.  

La référence *nulle* est désignée par le mot-clé **nil**. On peut envoyer des 
messages à *nil* sans provoquer d'erreur, seulement les valeurs retournées 
sont toujours 0. 

Les booléens ont le type **BOOL** et valent **YES** (1) ou **NO** (0).  

Les pointeurs sur fonction ont un équivalent. Ce sont les **sélécteurs**, de 
type **SEL**, qui permettent aussi l'envoi de messages : 
```
SEL monSel = @selector(changeName);
[john performSelector:monSel withObject:@"doe"];
// équivalent à 
[john changeName:@"dalton"];
```
  
Les objets chaines de caractères ObjC ont le type **NSString**, qui sont comme 
des chaines C (char * ) mais précédées de **@** : 
```
@"objet chaine ObjC"
"char * de C"
```

## Classes 

Les classes ObjC sont des *prototypes* que les instances suivent. Elles 
peuvent être dérivées.  
La classe **NSObject*** est prédéfinie et doit **absolument** être utilisée 
comme classe racine.  
On peut récupérer l'objet-classe d'une variable, qui est de type **Class** : 
```Class c = [unObjet class];```.  
On peut tester l'appartenance d'un objet à une classe : 
```
[objet1 isMemberOfClass:UneClasse]; // classe spécifiée
[objet2 isKindOfClass:UneClasse]; // classe spécifiée ou classe dérivée
```
L'instanciation sépare l'**allocation** (une *factory* statique créée 
automatiquement) de l'**initialisation** (une méthode d'instance) :  
```
// allocation
id ceb = [CompteBancaire alloc];
// initialisation
[ceb initWithEuros:100];
// une seule expression
id ceb = [[CompteBancaire alloc] initWithEuros:100];
``` 
  
Il n'y a pas de *variables de classe*, il faut utiliser les variables 
**static** offertes par le C.  
Pour initialiser une classe, la méthode **initialize** est appelée *au moins 
une fois* avant toute autre méthode. Il faut garantir qu'elle ne soit exécutée 
qu'une seule fois avec un idiome : 
```
+initialize {
  if (self == [MaClasse class]) {
    // initialisation
  }
}
```
  
On définit une classe ObjC en deux étapes : 

- l'**interface** (relation avec les autres classes, proriétés et sélécteurs)
```
// fichier Classe.h
#import "SuperClasse.h"
@interface UneClasse : SuperClasse
// propriétés
// sélécteurs
@end
```
- l'**implémentation** (variables d'instances *ivars*, code, synthèse des 
méthodes)
```
// fichier Classe.m
#import "Classe.h"
@implementation Classe 
{
  // variables d'instance
}
// méthodes
@ end
```

Les méthodes dont le sélécteur est précédé par le signe **+** sont statiques. 
Les méthodes de classe sont appelées directement sur l'objet-classe lui-même.   
Celles dont le sélécteur est précédé du signe **-** sont d'instance. 
```
#import <Foundation/Foundation.h>

@interface Classe : NSObject
+ (id)createWithValue:(int)v;
- (id)initWithValue:(int)v;
- (int)value;
@end
```
```
#import "Classe.h"
@implementation Classe
{
  int value;
}
- (id)initWithValue:(int)v {
  self = [super init];
  if (self) {
    NSLog(@"init");
    value = v;
  }
  return self;
}
- (int) value {
  return value;
}
+ (id)createWithValue:(int)v {
  return [[self alloc] initWithValue:v];
}
@end
```
```
#import <Foundation/Foundation.h>
#import "Classe.h"

int main (int argc, const char * argv [])
{
@autoreleasepool { // empêche les fuites 
  Classe *obj1 = [[Classe alloc] initWithValue:10];
  Classe *obj2 = [[Classe createWithValue:20]];
}
  return 0;
}
```
On note que : 

- **self** est un pointeur sur l'instance, typé avec sa classe de définition. 
En tant que variable, il peut être modifié. 
- **super** est un pointeur sur l'instance, typé avec la super-classe de sa 
définition. 
- *self* et *super* ont aussi un sens dans une méthode de classe, après tout 
les classes sont aussi des objets. 

## Propriétés 

Les **propriétés** sont des variables d'instance pour lesquelles des 
accesseurs sont définis. Elles sont déclarées dans une interface, un protocole, 
ou une catégorie.  
```@property (attributs) type nom;``` déclare les accesseurs 
```- (type)nom;``` et ```- (void)setNom:(type)newNom;```.  

On implémente une propriété avec **@synthesize**, **@dynamic**, ou à la main. 
```@synthesize nom, ..., nom1=nom2, ...;``` fait la synthèse automatique des 
propriétés nommées. *nom1=nom2* permet de synthétiser en utilisant le support 
de la variable *nom2*.  
**@dynamic** indique que les accesseurs seront fournis par un autre moyen 
(dynamique).  
*Note :* l'utilisation de **@synthesize** est optionnelle car la construction 
par défaut suffit en général. 

### Attributs 

- *getter=nom_méthode* permet de choisir le nom du getter
- *setter=nom_méthode* permet de choisir le nom du setter
- *readwrite* (par défaut) et *readonly* changent l'accès
- *strong* (implicite) donne la notion de possession de la propriété (on 
s'assure que la valeur existe tant qu'elle est assignée à la propriété)
- *weak*, au contraire, ne fait pas un lien fort
- *copy* fait une copie de l'argument avec de l'affecter. L'ancienne valeur 
est *release*
- *assign* fait une simple affectation
- *retain* : l'ancienne valeur est *release* et la nouvelle est *retain*
- *atomic* (par défaut) est thread-safe
- *nonatomic* n'est pas thread-safe

## Protocoles 

Un **protocole** correspond en gros à une interface : un contrat auquel on peut 
se conformer. 
```
// Printable.h
#import <Foundation/Foundation.h>
@protocol Printable <NSObject>
- (id)print;
@end
```
```
// Classe.h
@interface Classe : NSObject <Printable>
...
```
```
// Classe.m
#import "Classe.h"

@implementation Classe 
{
  int value;
}
...
- (id)print{
  NSLog(@"My value is d", self.value);
  return self;
}
...
@end
```

Dans les protocoles, on peut qualifier les sélécteurs dans des sections 
**@required** (par défaut, les classes ont l'*obligation* de définir les 
méthodes) ou **@optional** (aucune obligation).  
On peut tester ```[unObjet conformsToProtocol:@protocol(UnProtocole)];```.  
Un protocole peut en incorporer d'autres. 

## Blocs 

ObjC introduit les **blocs** : des fonctions anonymes avec liaison dans 
l'environnement (autrement dit des clôtures). 
```
int (^unBloc)(int) = ^int (int a) {
  return a+v;
};
// si v est globale, elle est capturée par référence, donc modifiable. 
// si v est locale (auto), elle est capturée comme const, donc non modifiable,
// à moins d'utiliser la directive __block dans sa déclaration. 
```

## Key Value Coding 

Le procotole informel (la catégorie) **NSKeyValueCoding** permet d'accéder à 
une propriété grâce aux noms des sélécteurs (entre autres) donnés sous forme 
de *NSString*. Il faut que la propriété ai les accesseurs adéquats. 
```
o.couleur = red;
// équivalent
[o setValue:red forKey:@"couleur"];
// ou
cours.osx.desc = @"cool";
// équivalent
[cours setValue:@"cool" forKeyPath:@"osx.description"];
```

## Les énumérations 

Elles suivent le protocole **NSFastEnumeration** et permettent d'énumérer 
rapidement les éléments d'une collection. Elles sont *safe*. On les emploie 
avec ```for (<type> <var> in <collection>) { ... }```

## Gestion mémoire 

### Comptage de référence (Manual Retain Release)

On associe à tout objet un compteur indiquant combien de pointeurs le désigne. 
Quand ce compteur tombe à 0, on supprime l'objet.  
Toutefois, ce n'est pas suffisant : il y a bien des références circulaires.  
*NSObject* a 3 façons de gérer le compteur : 

- *retain*, incrément le compteur de référence de l'objet
- *release*, décrémente le compteur et appelle **dealloc** sur l'objet si il 
tombe à 0
- *autorelease*, relâche la propriété mais délaye la suppression dans le futur. 
C'est utile si on veut créer un objet et le retourner, mais sans en garder la 
propriété

On est propriétaire d'un objet créée par *alloc, new, copy, mutable* et 
*mutableCopy*. On peut devenir propriétaire à tout instant avec **retain**

```
// créateur
id unObj = [[Classe alloc] init];
[client utilise:unObj];
...
[unObj release];
```
```
// client
- (void)utilise:(id)unObj {
  iVar = [unObj retain];
  ...
}
- (void)plusTard {
  if (iVar)
    [iVar release];
}
```