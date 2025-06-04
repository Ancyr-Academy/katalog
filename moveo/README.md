# Moveo

Moveo est une entreprise de covoiturage. Des covoitureurs y entrent des trajets et des passagers peuvent réserver des places dans ces trajets.
Le but de ce Kata est de développer la méthode de calcul de prix d'un trajet.

# Compétences développées 

- Test-Driven Development par incrément de valeur
- Domain-Driven Design avec un Domain Model riche
- Design Patterns (Strategy, Proxy, Adapter, Composite)
- Design Orienté Objet

# Exercices

## Niveau 1 - Prix simple

Le prix d'un trajet est calculé en fonction de la distance parcourue et du type de trajet (rural ou urbain).
- Le prix par kilomètre pour un trajet rural est de 1.05€
- Le prix par kilomètre pour un trajet urbain est de 1.25€

La méthode doit retourner le prix en euros.

```ts
class PriceCalculator {
  calculatePrice(data: {
    distance: number; // en km
    type: "rural" | "urban"
  }) {
    return 0;
  }
}
```

## Niveau 2 - Distance paramétrable

L'utilisateur peut préciser si la distance est en kilomètres ou en miles.

## Niveau 3 - Devise paramétrable

L'utilisateur peut préciser la devise dans laquelle il désire avoir le prix, en euros ou en dollars.
Utilisez un taux de conversion de 1.1 pour convertir les euros en dollars.
Par défaut, la devise est l'euro.

## Niveau 4 - Points de départ et d'arrivée

L'utilisateur ne fournit plus une distance mais deux points géographiques. La distance est calculée à partir de ces deux points.
L'utilisateur peut fournir le point de départ et le point d'arrivée soit : 
- En latitude et longitude
- En adresse postales

```ts
type Input = {
  type: "coordinates",
  departure: { latitude: number; longitude: number },
  arrival: { latitude: number; longitude: number }
} | {
  type: "address",
  departure: string, // adresse postale
  arrival: string // adresse postale
}
```

Un service externe (comme l'API de Google Maps) est utilisé pour calculer la distance entre les deux points.
Notez qu'appeler ce service externe n'est pas obligatoire, ce qui importe, c'est le design.

## Niveau 5 - Itinéraire

Le service externe utilisé pour calculer la distance entre les deux points ne fournit plus une distance mais un itinéraire.
Voilà un exemple d'input sortant de l'API :

```typescript
[
  {
    "type": "departure",
    "city": "Paris"
  },
  {
    "type": "urban-road", // route urbaine
    "distance": 411 // meters
  },
  {
    "type": "urban-road",
    "distance": 1443 // meters
  },
  {
    "type": "city-exit",
    "city": "Paris"
  },
  {
    "type": "highway-entry",
    "name": "A71",
  },
  {
    "type": "toll", // péage
    "price": 4.50,
    "currency": "EUR"
  },
  {
    "type": "highway", // autoroute (rural)
    "distance": 27 // kilometers
  },
  {
    "type": "highway-exit",
  },
  {
    "type": "city-entry",
    "city": "Bourges"
  },
  {
    "type": "urban-road",
    "distance": 4657 // meters
  },
  {
    "type": "arrival",
    "city": "Bourges"
  }
]
```

Notez que maintenant, le calcul du prix change : 
- Le prix par kilomètre sur l'autoroute est de 1.05€
- Le prix par kilomètre en zone urbaine est de 1.25€
- Rajouter le prix des péages

## Niveau 6 - Prix contextuel

Le prix au kilomètre varie selon la ville :
- A Marseille, le prix au kilomètre de route urbaine est de 1.50€
- A Lyon, le prix au kilomètre de route urbaine est de 1.40€
- A Paris, le prix est par tier. Les premiers 5km sont à 1.35€, les suivants à 1.15€
- A Bordeaux, le prix varie selon la période de l'année :
  - De Janvier à Juin, le prix est de 1.20€
  - De Juillet à Août, le prix est de 1.05€
  - De Septembre à Décembre, le prix est de 1.25€
- Dans toutes les autres villes, le prix est de 1.25€

## Niveau 7 - Promotion

Une promotion est appliquée selon la distance parcourue par le conducteur dans le mois : 
- 3% de réduction si le conducteur a parcouru plus de 500 km
- 5% de réduction si le conducteur a parcouru plus de 1000 km

S'il s'agit du premier trajet du conducteur, une promotion fixe de 5€ est appliquée.

## Niveau 8 - Conducteur Confirmés

Le prix au kilomètre en ville varie selon que le conducteur est confirmé ou non.

Les prix énoncés au Niveau 6 concernent les conducteurs non confirmés.
Pour les conducteurs confirmés, voici les prix :

- A Marseille, le prix au kilomètre de route urbaine est de 1.45€
- A Lyon, le prix au kilomètre de route urbaine est de 1.35€
- A Paris, le prix est fixé à 1.15€ par kilomètre, peu importe la distance
- A Bordeaux, le prix varie selon la période de l'année :
  - De Janvier à Juin, le prix est de 1.10€
  - De Juillet à Août, le prix est de 0.95€
  - De Septembre à Décembre, le prix est de 1.05€
- Dans toutes les autres villes, le prix est de 1.25€

## Niveau 9 - Trajets Annuels

Dans le cas d'un trajet qui passe par Paris, une promotion de 25% est appliquée si le conducteur a parcouru plus de 10 000 km dans l'année.
La promotion ne s'applique que sur le segment de route qui passe par Paris.

