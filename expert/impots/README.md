# Système Fiscal de Borginie

Dans le pays imaginaire de Borginie, on vous demande développer un système qui permet de simuler le calcul de l'impôt sur le revenu.

Il s'agit de développer un classe (ou une fonction) qui retourne simplement le montant de l'impôt à payer selon la situation de la personne.

Pour information, la monnaie locale de Borginie est le Borgi.

```ts 
class TaxSystem {
  calculateTax(): number {
    // Return a value
  }
}
```

L'impôt à payer dépend du revenu imposable de la personne et repose sur un système de tranches.

- Jusqu'à 10 000 Borgis, l'impôt est de 0%
- De 10 001 à 20 000 Borgis, l'impôt est de 10%
- de 20 001 à 30 000 Borgis, l'impôt est de 18%
- De 30 001 à 50 000 Borgis, l'impôt est de 25%
- Au delà de 50 000 Borgis, l'impôt est de 30%

Le calcul de l'impôt se fait sur le revenu imposable de l'année précédente.

## Niveau 1 - Calcul de l'impôt

Tous les citoyens de Borginie sont des salariés et ont une fiche de paie `PaySlip` qui indique leur revenu imposable.
Utilisez cette information pour calculer l'impôt à payer.

```ts
class TaxSystem {
  calculateTax(paySlip: number): number {
    return 0; // Replace with the actual calculation
  }
}
```

## Niveau 2 - Prélèvement à la source

Certains salariés ont opté pour le prélèvement à la source et paient déjà une partie de leur impôt chaque mois. 
Retranchez l'impôt déjà payé du montant à payer.

## Niveau 3 - Réductions d'impôts

Certains salariés peuvent bénéficier de réductions d'impôts.
Certaines réductions sont fixes (par exemple, 500 Borgis) et d'autres sont proportionnelles au revenu imposable (par exemple, 5% du revenu imposable).
Représentez cette notion dans le calcul de l'impôt.

Pour information, le flux de calcul de l'impôt est le suivant :
- Calcul de l'impôt brut
- Retranchement de l'impôt déjà payé
- Application des réductions d'impôts

## Niveau 4 - Déductions d'impôts conditionnelles

Certaines déductions d'impôts sont conditionnelles. Par exemple, elles ne s'applique que si le revenu imposable (avant toutes déductions) 
est inférieur à un certain seuil.

## Niveau 5 - Plafonnement des réductions d'impôts

Les réductions d'impôts sont plafonnés à 3 871 Borgis. L'accumulation de toutes les réductions d'impôts ne peut pas dépasser ce montant.

## Niveau 6 - Entreprenariat

Certains citoyens de Borginie, en plus d'être salariés, sont également entrepreneur individuel. 
Les revenus de leur activité d'entrepreneur sont également soumis à l'impôt sur le revenu, mais avec un taux différent.
Il existe deux types d'activité : les prestations de services et les activités commerciales.

Pour calculer le montant pris en compte dans l'impôt à payer, il faut appliquer un abattement.
- de 34% pour les prestations de services
- de 71% pour les activités commerciales

Un exemple : si l'entrepreneur a gagné 10 000 Borgis en prestations de services, seul 6 600 Borgis seront pris en compte dans le calcul de l'impôt.
Autre exemple : si l'entrepreneur a gagné 10 000 Borgis en activités commerciales, seul 2 900 Borgis seront pris en compte dans le calcul de l'impôt.

Le flux de calcul de l'impôt est le suivant :

Pour information, le flux de calcul de l'impôt est le suivant :
- Calcul de l'impôt brut (salaire + revenus d'entrepreneur)
- Retranchement de l'impôt déjà payé
- Application des réductions d'impôts

## Niveau 7 - Actionnaires

Certains citoyens de Borginie sont également actionnaires et perçoivent des dividendes.
Lorsqu'un citoyen perçoit des dividendes, il doit payer un impôt fixe de 30% sur le montant des dividendes perçus.

Donc si un citoyen perçoit 1 000 Borgis de dividendes, il doit payer 300 Borgis d'impôt.

Notez qu'il s'agit de payer 300 Borgis d'impôts peu importe le montant de l'impôt déjà payé ou les réductions d'impôts.
Ce montant est fixe et ne dépend pas du revenu imposable.

Pour information, le flux de calcul de l'impôt est le suivant :
- Calcul de l'impôt brut (salaire + revenus d'entrepreneur)
- Retranchement de l'impôt déjà payé
- Application des réductions d'impôts
- Ajout de l'impôt sur les dividendes

## Niveau 8 - Réductions Locales

Pour encourager l'activité entreprenariale, la Borginie a mis en place un système de réduction d'impôts locales.
Ces réductions ne sont appliqués qu'au revenu issus de l'activité d'entrepreneur.

Voici les règles selon les villes de Borginie :
- Amb : l'abattement forfaitaire est de 50% pour les prestations de services et de 90% pour les activités commerciales
- Tabhati : aucun impôt pendant les 3 premières années d'activité
- Borginopolis : l'abattement forfaitaire dépend à la fois du nombre d'année d'activité et du type d'activité :
  - Pour les prestations de services, l'abattement est de 100% la première année, 70% la deuxième année, 34% les années suivantes.
  - Pour les activités commerciales, l'abattement est de 100% la première année, 90% la deuxième année, 71% les années suivantes.

## Niveau 9 - Impots supplémentaires

Les citoyens de Borginie doivent également payer un impôt sur :

**Les cryptomonnaies** : un impôt fixe de 25% est appliqué sur les gains réalisés lors de la vente de cryptomonnaies.

**Investissements immobiliers** : un impôt fixe de 17% est appliqué sur les revenus locatifs. Au delà de 20 000 Borgis de revenus locatifs, l'impôt est de 25%. 
Notez que le pourcentage n'est pas progressif. Sur 15 000 Borgis de revenus locatifs, l'impôt est fixé à 17% du total mais sur 25 000 Borgis, l'impôt est de 25% du total.