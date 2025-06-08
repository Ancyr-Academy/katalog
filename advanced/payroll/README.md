# Payroll

Ce cas d'étude est largement inspiré de celui de Robert C. Martin dans son livre Agile Software Development.
Il s'agit d'une application complète de gestion de la paie d'une entreprise.
L'application est utilisée par un administrateur système pour gérer ses employés. 

## Fonctionnement

- Certains employés sont payés à l'heure à un taux horaire fixe qui varie d'un employé à l'autre. Chaque employé à son taux horaire.
- Ces employés soumettent chaque jour un rapport d'heures travaillées.
- Les employés payés à l'heure doivent travailler 8h par jour. Toute heure supplémentaire est payée à 1.5 fois le taux horaire.
- Ils sont payés chaque Vendredi.
- Certains employés ont un salaire fixe mensuel, ils sont payés une fois par mois.
- Certains salariés sont payés à la commission. Ils touchent un pourcentage sur les ventes qu'ils réalisent. Le pourcentage varie d'un employé à l'autre.
- Ils soumettent chaque jour un rapport de toutes les ventes réalisées ce jour là.
- Ils sont payés chaque Vendredi.
- Chaque employé peut avoir une méthode de paiement différente
  - Certains recoivent un chèque directement remis en main propre
  - D'autres reçoivent un virement bancaire
  - D'autres reçoivent le paiement par courrier
- Certains employés partie d'un syndicat. Ils doivent verser une cotisation syndicale mensuelle. Cette cotisation est fixe et est déduite de leur salaire.
- S'ils font partie du syndicat, ce syndicat peut également rajouter des charges ponctuelles qui sont elles aussi déduites du salaire.
- L'application est lancée une fois par jour. Il génère un rapport de paie pour chaque employé.

## Compétences développées

- Test-Driven Development par incrément de valeur
- Clean Architecture & Architecture Logicielle
- Design Orienté Objet
- Pattern Strategy