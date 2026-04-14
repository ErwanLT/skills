---
name: dev-app-java
description: "Développement d’applications Java de bout en bout (création, structuration, refactor, revue qualité) pour Java classique, Spring Boot ou Quarkus. Utiliser dès qu’il s’agit d’écrire une application Java, de générer un projet, d’appliquer les bonnes pratiques (Javadoc, tests unitaires, design patterns), ou de guider l’architecture et la qualité du code."
---

# Dev App Java

## Overview

Appliquer des bonnes pratiques Java transverses (Javadoc, tests unitaires, design patterns) sur du code existant ou du nouveau code, quel que soit le framework.

## Workflow

1. **Clarifier le contexte**
   - Identifier le type d’app (Java classique, Spring Boot, Quarkus) et le but immédiat.
   - Si le framework est mentionné, respecter ses conventions, sans supposer d’API spécifiques.

2. **Diagnostiquer le besoin**
   - Javadoc: manque de contrats, ambiguïtés, exceptions, ou exemples.
   - Tests: couverture de cas, isolation, assertions faibles, tests fragiles.
   - Design patterns: duplication de logique, responsabilités mélangées, besoin d’extensibilité.

3. **Appliquer les bonnes pratiques ciblées**
   - Utiliser les règles synthétiques ci-dessous et la référence détaillée.
   - Proposer des changements minimaux, sûrs, et justifiés.

4. **Valider**
   - Vérifier lisibilité, cohérence, et impact sur le reste du code.
   - Proposer des tests unitaires associés si un comportement change.

## Javadoc

- Rédiger un contrat clair: rôle, préconditions, postconditions, invariants.
- Documenter les paramètres, valeurs de retour, exceptions (y compris conditions).
- Éviter la redondance avec le nom de la méthode: expliquer le “pourquoi”.
- Ajouter un exemple si l’usage n’est pas évident.
- Mettre à jour la Javadoc lors de tout changement de signature ou de comportement.

**Quand agir:** méthode publique sans contrat explicite, comportements implicites, erreurs possibles mal documentées.

## Tests Unitaires

- Utiliser JUnit 5 par défaut; isoler les unités (mocks/stubs) si nécessaire.
- Couvrir les cas nominaux, limites, erreurs, et invariants.
- Éviter les tests fragiles (dépendances temporelles, ordre, données globales).
- Préférer des assertions précises et lisibles; nommer les tests par comportement.
- Minimiser la duplication via helpers/factories si utile.

**Quand agir:** absence de tests, couverture faible des cas limites, tests floconneux.

## Design Patterns

- Introduire un pattern seulement si cela simplifie ou stabilise le design.
- Préférer la composition à l’héritage.
- Patterns fréquents:
  - **Strategy**: variantes d’algorithmes ou règles.
  - **Factory**: construction complexe ou multiples implémentations.
  - **Adapter**: intégration d’API externes.
  - **Decorator**: responsabilités additionnelles sans explosion de classes.
- Justifier le pattern choisi par un problème concret.

**Quand agir:** logique conditionnelle répétée, classe “god object”, extension difficile.

## Références

- Pour des règles détaillées, exemples, et checklists, lire `references/java-best-practices.md`.
- Si le projet est Spring Boot, lire `references/spring-boot.md`.
- Si le projet est Quarkus, lire `references/quarkus.md`.
