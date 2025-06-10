# Jeu du Bowling

Ecrivez un programme pour calculer le score d'une partie de bowling.
L'utilisateur peut saisir le score de chaque lancer de la partie.

Les règles sont les suivantes :
- Une partie se compose de 10 frames.
- Dans chaque frame, le joueur a deux occasions de renverser 10 quilles.
- Le score pour la frame est le nombre total de quilles renversées, plus des bonus pour les strikes et les spares.
- Un spare est lorsque le joueur renverse toutes les 10 quilles en deux essais. Le bonus pour cette frame est le nombre de quilles renversées lors du lancer suivant.
- Un strike est lorsque le joueur renverse toutes les 10 quilles lors de son premier essai. Le bonus pour cette frame est la valeur des deux prochains lancers.


# Astuce

Créez une classe `Game` avec les méthodes suivantes :

```ts
class Game {
  roll(number: number) {
    
  }

  score() {
    return 0;
  }
}
```

# Bonus

Cet exercice peut être réalisé en exactement 5 tests.
