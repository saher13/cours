## Gestion des conflits

### Reduce/reduce

En cas de conflit reduce/reduce, la regle apparaissant en premier dans le fichier est prioritaire.

### Shift/reduce

En cas de conflit shift/reduce, Menhir compare la precedence eet l'associativite du token à lire avec celles de la règle permettant la réduction. Si elle n'est pas redifinie par la directive `%prec`, la precedence d'une regle est celle de son terminal le plus à droite, ou indefinie si elle n'a pas de terminal.

En cas d'egalite au niveau de la precedence, Menhir regarde l'associativité du token à lire.
* left : reduce
* right : shift
* nonassoc : syntax error

### Remaque

L'associativite à gauche ou à droite n'a de sens que pour les operateurs `op` du type `x op x -> op`, ou autrement dit les opérateur binaires. On règle les autre conflits en définissant des règles de priorité adequates.

Une associativite déclaree avant une autre (dans le fichier) sera MOINS prioritaire que cette derniere.

On peut declarer plusieurs associativites sur un même ligne, les tokens auront tous le meme niveau de priorite.