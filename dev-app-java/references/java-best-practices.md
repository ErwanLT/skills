# Java Best Practices (Javadoc, Tests, Patterns)

## Table of Contents
- Javadoc Rules
- Unit Testing Rules
- Design Pattern Guidance
- Review Checklist
- Example Requests

## Javadoc Rules

- Documenter le contrat fonctionnel: ce que fait la méthode, pas comment.
- Décrire les paramètres avec contraintes (nullabilité, formats, unités).
- Décrire le retour (sens métier, valeurs sentinelles).
- Lister les exceptions et conditions de déclenchement.
- Ajouter un exemple d’utilisation si la méthode est non triviale.
- Éviter la répétition avec le nom de la méthode; expliquer les effets de bord.

Exemple minimal:
```
/**
 * Calcule le total TTC à partir d’un montant HT.
 * @param amountHt montant HT, en euros, strictement positif
 * @param tvaRate taux de TVA entre 0 et 1
 * @return total TTC arrondi à 2 décimales
 * @throws IllegalArgumentException si amountHt <= 0 ou tvaRate hors limites
 */
```

## Unit Testing Rules

- Utiliser JUnit 5 (`@Test`, `@BeforeEach`), Mockito si nécessaire.
- Structure recommandée: Arrange / Act / Assert.
- Cas à couvrir:
  - Nominal (happy path)
  - Bords (limites, formats, nulls)
  - Erreurs (exceptions attendues)
  - Invariants (propriétés qui doivent toujours tenir)
- Éviter:
  - Accès réseau/fichier dans unitaire
  - Dépendances statiques globales
  - Assertions faibles (ex: `assertTrue(obj != null)`)

## Design Pattern Guidance

- Ne pas introduire de pattern si la solution simple est claire et stable.
- Indications typiques:
  - Multiples branches conditionnelles basées sur un type ou une règle: Strategy.
  - Construction d’objets complexes ou variantes de produits: Factory/Builder.
  - Adaptation d’une API tierce: Adapter.
  - Ajout dynamique de comportement: Decorator.
- Vérifier l’impact sur la lisibilité et le coût de maintenance.

## Review Checklist

- Javadoc
  - Contrat explicite et à jour
  - Exceptions documentées
  - Paramètres/retour clairs
- Tests
  - Cas limites couverts
  - Assertions significatives
  - Pas de dépendances externes
- Design
  - Responsabilités cohérentes
  - Complexité maîtrisée
  - Pattern justifié par un besoin réel

## Example Requests

- "Ajoute la Javadoc manquante à ces méthodes publiques"
- "Propose des tests unitaires pour cette classe"
- "Refactorise ce bloc en utilisant un pattern approprié"
