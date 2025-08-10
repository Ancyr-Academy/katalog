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

Par exemple, si le revenu du citoyen est de 28 000 Borgis
- Les 10 000 premiers Borgis ne sont pas imposés
- Les 10 000 suivants sont imposés à 10%
- Les 8 000 suivants sont imposés à 18%

Notez bien que l'imposition s'applique sur la tranche concernée.

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

Vous représentez le système des impôts, donc il s'agit d'une information que vous avez déjà dans votre base de données. 
Utilisez l'ID de l'utilisateur pour faire la liaison entre le montant déjà réglé et l'utilisateur.

```ts 
interface PaymentRepository {
  // Methods
}

class TaxSystem {
  private readonly paymentsRepository: PaymentRepository;
}
```

## Niveau 2.1 - Rapport complet

Le système ne doit plus seulement retourner un simple nombre mais un rapport un peu plus complet du calcul :
- La base imposable retenue
- Le montant déjà payé
- Le montant restant à payer

La base imposable est la somme des revenus soumis à imposition, soit les revenus du citoyen au delà des 10 000 Borgis
(puisque les 10 000 premiers Borgis ne sont pas imposables)

### Note

Notez que l'interface de la classe TaxCalculator évolue beaucoup. **Si vous développez en TDD/BDD, vous avez probablement
dû mettre à jour beaucoup de tests à chaque niveau.**

Et ça ne va pas s'améliorer avec les niveaux à venir. Que pourriez-vous faire pour réduire l'impact de l'évolution du
design sur vos tests ? :)

## Niveau 3 - Réductions d'impôts

Certains salariés peuvent bénéficier de réductions d'impôts. Ces réductions sont fournis en paramètre d'entrée du `TaxCalculator`.
Cette réduction d'impôt est directement soustraite du montant total à payer.

Il existe deux types de réduction : 
- **Les réductions fixes** : par exemple, 100 Borgis.
- **Les réductions au prorata** : par exemple, 20% de l'impôt à payer.

Pour information, le flux de calcul de l'impôt est le suivant :
- Calcul de l'impôt brut
- Retranchement de l'impôt déjà payé
- Application des réductions d'impôts

Notez que les réductions ne peuvent aboutir à un impôt négatif.

Exemple : si le citoyen doit payer 300 Borgis mais bénéficie d'une réductiode 500 Borgis, il paiera 0 Borgis mais il ne 
sera pas remboursé de 200 Borgis (la différence entre 500 et 300).

Le rapport doit également mentionner le montant total des réductions appliquées.

## Niveau 3.1 - Prorata maximale

Il n'est désormais possible d'appliquer qu'une seule réduction au prorata. Si plusieurs sont fournies, la plus élevée
est prise en compte.

De plus, cette réduction doit-être appliquée en premier, avant les réductions fixe.

## Niveau 4 - Déductions d'impôts conditionnelles

Les déductions d'impôts sont conditionnelles. 

- Certaines déductions s'appliquent en toutes circonstances
- D'autres ne s'appliquent que si le montant à payer est supérieur à un seuil
- D'autres uniquement si la base imposable est inférieure à un seuil

## Niveau 5 - Plafonnement des réductions d'impôts

Les réductions d'impôts sont plafonnés à 1 271 Borgis. L'accumulation de toutes les réductions d'impôts ne peut pas dépasser ce montant.

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