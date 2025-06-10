# Le Pendu

Ecrivez un programme dans lequel l'utilisateur doit deviner un mot, lettre par lettre.
L'utilisateur peut échouer 6 fois.
Si l'utilisateur tape une lettre qui a déjà été révélée, le programme ne doit pas compter cela comme une erreur.

Vous devez finir avec un fichier `main.ts` contenant le code suivant :

```ts 
function main() {
  const game = new Game();
  return game.start();
}

main();
```

NOTE : le jeu aura probablement des paramètres de constructeur, ils émergeront au fur et à mesure de votre développement, 
à moins que vous ne sachiez déjà lesquels vous avez besoin.


# Hints

Vous aurez besoin d'au moins trois collaborateurs :
- Un affichage pour montrer les informations à l'utilisateur
- Une entrée pour obtenir la lettre de l'utilisateur
- Un générateur de mots

