# Opera Kata

L'opéra de Paris vous demande de réaliser une application permettant à son personnel de gérer la
réservation de places d'opéra.

Dans cet opéra, il y a 3 types emplacement dans lesquels on peut réserver des places :
- **L'Orchestre**, organisé en 12 rangées de 12 places
- La **Corbeille**, organisée en 7 rangées de 12 places
- Le **Balcon**, dont la topologie diffère un peu : elle est organisée en 5 rangées de 10 places, mais également de 12 loges.
  Ces loges sont chacune composées de 5 places.

# Compétences développées

- Test-Driven Development par incrément de valeur
- Domain-Driven Design avec un Domain Model riche
- Design Patterns (Strategy)
- Design Orienté Objet

# Exercices

## Niveau 1

### Description

Lorsqu'un usager se présente au comptoir, il peut informer le personnel de la réservation qu'il désire faire. L'algorithme
se charge ensuite de trouver les places correspondant à sa demande.

Sa demande contient :
- le nombre de places qu'il souhaite, entre 1 et 5
- l'emplacement, soit `Balcon`, `Orchestre` ou `Corbeille`
- une préférence entre `avant` ou `arrière` pour l'orchestre, la corbeille et les rangées du Balcon
- S'il choisit Balcon, il peut préciser s'il veut absolument une loge ou non. S'il désire une loge, alors l'algorithme doit
lui servir une loge à condition qu'une loge soit disponible. Sinon, l'algorithme doit chercher dans les rangées du balcon,
puis dans les loges s'il n'y a aucune place disponible dans les rangées.

Si l'usager désire plusieurs places, ces places doivent être côte à côte et dans la même rangée.
Dans le cas des loges, elles doivent simplement être dans la même loge.

L'algorithme cherche les places correspondante et imprime un ticket de réservation qui détaille les places réservées.

Ce ticket contient : 
- Le nombre de place réservées
- Pour chaque place
  - L'emplacement (Orchestre, Corbeille, Balcon)
  - S'il s'agit d'un siège dans une rangée, le numéro de la rangée et le numéro du siège
  - S'il s'agit d'un siège dans un balcon, le numéro du balcon et le numéro du siège

Les places sont ensuite marquées comme réservées et ne peuvent pas être réservées à nouveau.

Pour fonctionner, l'algorithme doit récupérer la topologie de la salle en base de données. Cette topologie contient l'ensemble
de l'organisation de l'Opéra avec son Balcon et ses loges, son Orchestre et sa Corbeille. Elle contient également les sièges et 
leur disponibilité.

Actuellement, il n'y a qu'une seule topologie globale au fonctionnement de l'Opéra. Plus tard, on devra maintenir une topologie
par pièce jouée.

Si aucune place n'a été trouvé, alors le programme imprime un message d'erreur.

## Niveau 2

Quelques contraintes viennent restreindre l'algorithme de recherche :
- Pour ce qui est du balcon, **au moins 2 loges doivent rester disponibles pour les VIPs**. S'il ne reste plus que deux loges, alors
  l'algorithme ne doit plus servir de loge.
- Pareillement, la première rangée de l'Orchestre (la plus proche de la scène) **est également réservée aux VIPs**.
- Les 2 premières rangées de la corbeille sont réservées à un programme de culture chez les jeunes qui sont gérées différemment.
Ce programme a lieu du 21 Juin au 21 Septembre, et donc ces places sont indisponibles entre ces dates.

## Niveau 3

Différentes pièces sont à l'affiche, et donc chaque pièce doit posséder sa propre topologie d'organisation et de réservation.
Maintenant, lorsqu'un client souhaite réserver, il doit également renseigner la pièce.

Chaque pièce possède, en plus de sa propre topologie, sa propre configuration : 
- Certaines pièces ne sont pas éligible au programme de culture chez les jeunes, auquel cas les places de la corbeille sont toutes disponibles.
- Pour certaines pièces, deux nouveaux étages sont accessible et elles sont composées uniquement de loges, 12 loges par étage, 5 places par loge.
- Pour certaines pièces, les loges du Balcon sont toutes réservées aux VIPs.
- Pour certaines pièces, la première rangée de l'Orchestre n'est **PLUS** réservée aux VIPs.
- Pour certaines pièces, une loge n'est réservable que si la totalité de la loge est réservée

# Notes

L'ensemble de ce programme peut (et doit) être développé sans se soucier de la base de données. Mais il sera
nécessaire de l'intégrer à un moment donné. 

Le but est d'empêcher le design de la base de données d'influencer le design du code, et de repousser l'intégration
de la base de données le plus tard possible.