# Guidelines TypeScript

Statut : Recommendation (REC)

Cette présente convention rassemble les bonnes pratiques TypeScript, que nous souhaitons retrouver dans vos projets. Elle a pour but d’évoluer dans le temps et de s’adapter à chaque nouveau projet.

⚠️Notez que ceci s’inscrit dans la suite logique des [recommandations JavaScript](https://github.com/hepl-dcc/dcc-guidelines), qui sont une étape préliminaire à ceci.

## Nommage

- Utilisez la notation *PascalCase* pour les noms des `type`.
- Utilisez la notation *PascalCase* pour les valeurs d’`enum`.
- Préfixez les noms d’interfaces avec un `I`.
- Utilisez la notation *camelCase* pour les noms des fonctions.
- Utilisez la notation *camelCasel* pour les noms des propriétés et variables locales.
- Préfixez vos propriétés privées par un `_`.
- Utilisez des mots entiers dans les noms lorsque cela est possible. Évitez les abréviations abusives. Il s’agit du code source, celui à destination des humains.

### Fichier

- Utilisez l’extension `.ts` pour les fichiers TypeScript.
- Les noms des fichiers TypeScript doivent être en *PascalCase*.

## Déclaration

- N’exportez que les types ou fonctions, s’ils sont partagés entre plusieurs composants.
- N’introduisez pas de nouveaux types ou valeurs dans l’espace de noms global. Dans un fichier, les définitions de type doivent venir en premier.

## Opérateurs

- Utilisez les opérateurs `===` et `!==` au lieu de `==` et `!=`.
- Terminez vos instructions par un `;`. Ne pas utiliser de point-virgule est source de confusion et d’erreurs.
- Pour définir un objet préférez les accolades `{}` au lieu de `new Object()`.
- Pour déclarer un tableau utilisez les crochets `[]` au lieu de `new Array()`.
- Utilisez les *arrow functions* plutôt que les  *fonctions anonymes*.
- N’utilisez jamais `var`, mais plutôt `const` lorsque c’est possible, sinon utilisez `let`.
- Évitez les affectations en chaine ou multiples lors de la déclaration.

## Points de sortie

- Ne vous privez pas de sortir d’une fonction dès que possible :

  ```js
  // Booof !!
  public getFirstUser(): Person | undefined {
  let firstUser?: Person;
  if (this.hasUsers()) {
    if (!this.usersValidated()) {
      if (this.validateUsers())
        firstUser = this.getUser(0);
    } else {
        firstUser = this.getUser(0);
    }
  }
   return firstUser;
  }
  
  // Mieux !!
  public getFirstUser(): Person | undefined {
    if (!this.hasUsers())
      return undefined;
  
    if (!this.usersValidated() && !this.validateUsers())
        return undefined;
  
    return return this.getUser(0);
  }
  
  // Encore mieux ! !! Pour un cas simple comme celui-ci qui consiste à choisir entre 2 valeurs de retour
  // bad!!
  public getFirstUser(): Person | undefined {
    const userInvalid = !this.hasUsers() || (!this.usersValidated() && !this.validateUsers());
    return userInvalid ? undefined : this.getUser(0);
  }
  ```
  
- Définissez toujours explicitement un type de retour pour les méthodes qui font plus d’une ligne.

## Retours à la ligne

Les écrans des programmeurs sont souvent plus larges que hauts. Il est courant que les largeurs soient d’au moins 120 colonnes, mais que les hauteurs soient inférieures à 100. Par conséquent, pour utiliser au mieux la surface de l’écran, il est souhaitable de préserver l’espace vertical de l’écran dans la mesure du possible.

- Certaines bases de code préconisent de rompre les lignes à 80 colonnes. Avec les tailles d’écran actuelles, c’est moins nécessaire. Ne rompez pas les lignes avant 120 colonnes.
- Ne gaspillez pas des lignes vides inutilement. Par exemple, la première ligne d’une fonction ne doit pas être une ligne vide.
- Ne mettez pas chaque importation dans une déclaration d’importation séparée. Essayer, si c’est possible, de les rassembler. Il existe des plugins, dans les éditeurs, qui vous permettent de simplifier les importations.

## Gestion des erreurs

Utilisez à bon escient les exceptions. Préférez lancer des exceptions lorsque des comportements inattendus surviennent. Il s’agit d’une gestion des erreurs plus expressive qu’un code d’erreur. Il y a différents avantages. En voici quelques-uns :

- Les exceptions peuvent éviter d’avoir des paternes du type `if error status then return`.
- Les exceptions peuvent contenir plus d’informations qu’un simple code de retour.
- Les codes d’erreur peuvent être ignorés, mais pas les exceptions. Si la couche immédiate ne traite pas l’exception, elle sera remontée à la couche externe.
- La propriété optionnelle `message` d’une `Error` devrait, si elle est définie, contenir un message de débogage en anglais qui n’est pas destiné à l’utilisateur final.
