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

Ce kata long et complexe est divisée en 2 sections. Vous pouvez vous arrêter à la fin de la première section, mais il est
encouragé de faire les deux.

# Section 1 - Salariés

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

# Section 2 - Entrepreneurs

## Niveau 6 - Entreprenariat

Certains citoyens de Borginie, en plus d'être salariés, **sont également entrepreneurs**. 
Les revenus de leur activité d'entrepreneur sont également soumis à l'impôt sur le revenu.

Le fonctionnement est le suivant :
- L'entrepreneur déclare ses revenus issus des activités entreprenarial **en paramètre d'entrée du système**
- Un abattement forfaitaire de 34% est appliqué, c'est à dire que **seulement 66% de ces revenus sont imposables**
- Après abattement, le revenu est ajouté à son revenu total
- Le reste du calcul reste le même

## Niveau 6.1 - Types d'entreprenariats

Il existe deux types d'activité : **les prestations de services** et **les activités commerciales**.
L'utilisateur doit à la fois entrer la somme de ses revenus relevant des prestations de services et celle relevant
de ses activités commerciales, car l'abattement forfaitaire n'est pas le même.

- Dans le cas de la **prestation de service**, l'abattement reste de 34%
- Dans le cas **d'activité commerciale**, l'abattement est de 71%

## Niveau 6.2 - Années d'activités

Tous les entrepreneurs **sont exempts d'impôts la première année d'activité**. 

Vous avez déjà dans votre base de données les informations sur les citoyens de Borginie, il vous suffit de la récupérer avec un repository.

Notez que la déclaration de l'entreprise **ne peut survenir que l'année suivant sa création.** Si elle a été créée en 2023, elle 
ne peut pas participer à la déclaration des revenues 2023 (pour l'année 2022).

## Niveau 6.3 - Activités multiples

Les entrepreneurs peuvent **cumuler plusieurs activités entreprenariales distinctes**. Chaque activité est soumis à son
propre abattement car elles peuvent avoir un nombre différent d'années d'activités

L'utilisateur doit déclarer les revenus pour chacune de ses entreprises. Si un revenu manque, le formulaire est rejeté
et doit-être recommencé.

L'utilisateur fourni également l'ID de l'entreprise en plus de la déclaration de revenues.

## Niveau 6.4 - Revenues multiple par activités

Une même activité peut **déclarer plusieurs types de revenues.**
Une entreprise peut donc déclarer un revenu commercial ainsi qu'un revenu de prestation de service.

## Niveau 7 - Actionnaires

Les citoyens de Borgini peuvent également avoir des entreprises qui sont soumis à un régime d'imposition différent. 
Les détails de ce régime ne sont pas gérés dans notre système. Cependant, ces citoyens peuvent percevoir des dividendes
de la part de ces entreprises.

Ils doivent donc déclarer les dividendes qu'ils ont perçu, et ce par entreprise.

30% de ces dividendes se rajoutent à l'impôt final, sans être éligible à une quelconque réduction d'impôt.

Ainsi, si un citoyen touche 1 000 Borgi de dividendes, il devra payer 300 Borgi d'impôts, inconditionnellement.

Pour rappel, le flux de calcul de l'impôt est le suivant :
- Calcul de l'impôt brut (salaire + revenus d'entrepreneur)
- Retranchement de l'impôt déjà payé
- Application des réductions d'impôts
- Ajout de l'impôt sur les dividendes

## Niveau 8 - Règles locales

Pour encourager l'activité entreprenariale, la Borginie a mis en place un système de calcul d'impôt local, ce qui signifie qu'il 
est plus avantageux d'entreprendre dans certaines villes que dans d'autres. 

Notez que chaque entreprise peut-être localisée dans une ville différente. **C'est la ville de l'entreprise qui détermine l'impôt, pas la ville de résidence du citoyen**.

### Astuce

Analysez la variabilité, c'est-à-dire ce qui varie d'une ville à l'autre. Analysez également les informations dont vous avez besoin
pour prendre des décisions dans chaque cas.

## Niveau 8.1 - Abattement forfaitaire

Actuellement, l'abattement forfaitaire est de 34% pour les prestations de service et 71% pour les prestations commerciales.
Désormais, ce taux varie selon la ville où se situe l'entreprise : 

- **Amb** : 25% pour les prestations de service et 51% pour les prestations commerciales
- **Tabhati** : 100% d'abattement (aucun impot) pendant les 3 premières années d'activité
- **Borginopolis** : règle avancée
  - **Prestations de services** : 100% d'abattement la première année, 70% la 2e, puis 34% les années suivantes
  - **Prestations commerciales** : 100% d'abattement la première année, 87% la 2e, puis 71% les années suivantes
- **Autres villes** : 34% pour les prestations de service, 71% pour les prestations commerciales

## Niveau 8.2 - Dividendes

Le même principe s'applique pour les dividendes 

- **Amb** : 25% de dividendes
- **Tabhati** : aucun impot sur les dividendes pendant les 3 premières années puis 30% les années suivantes
- **Borginopolis** : 15% sur les 40 000 premiers Borgis, puis 40% sur les autres
- **Autres villes** : 30% 