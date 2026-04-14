# Spring Boot Best Practices (Tests, Patterns)

## Table of Contents
- Testing
- Architecture & Layers
- Common Patterns
- Transactions & Persistence
- API & OpenAPI
- Review Checklist

---

## Testing

- Tester les couches en isolation :
  - `@WebMvcTest` pour les contrôleurs MVC
  - `@DataJpaTest` pour la couche JPA
  - Tests unitaires purs pour les services

- Utiliser `@SpringBootTest` uniquement pour les tests d’intégration complets
- Éviter d’en faire le choix par défaut (coût élevé)

- Utiliser **Testcontainers** pour les dépendances externes (DB, Kafka)
- Configurer des profils dédiés (`@ActiveProfiles("test")`)

- Séparer clairement :
  - tests unitaires (rapides)
  - tests d’intégration (plus lents)

- Structurer en **Given / When / Then**
- Tester le comportement, pas l’implémentation

**Quand agir :**
- Tests trop lents ou instables
- Usage excessif de `@SpringBootTest`
- Faible couverture des services

---

## Architecture & Layers

- Séparer clairement :
  - **Controller** → exposition HTTP
  - **Service** → logique métier
  - **Repository** → accès aux données

- Les contrôleurs doivent rester **fins**
- La logique métier appartient aux services
- Les repositories ne contiennent que de la logique d’accès aux données

- Ne pas mélanger logique métier et technique

- Favoriser des services cohérents (une responsabilité claire)

**Quand agir :**
- Logique métier dans les contrôleurs
- Services trop volumineux
- Couplage fort entre couches

---

## Common Patterns

- Injection par **constructeur** (recommandé)
  - Favorise l’immuabilité et la testabilité

- Utiliser des **DTOs** pour l’exposition API
- Mapper explicitement DTO ↔ domaine

- Préférer la composition à l’héritage

- Patterns fréquents :
  - **Strategy** : variantes métier
  - **Factory** : création complexe
  - **Adapter** : intégration externe

- Utiliser `@Configuration` pour centraliser les configurations

**Quand agir :**
- Code difficile à tester
- Duplication de logique
- Couplage avec des API externes

---

## Transactions & Persistence

- Gérer les transactions au niveau **service** (`@Transactional`)
- Ne jamais gérer de transaction dans un contrôleur

- Être explicite sur :
  - lecture seule (`readOnly = true`)
  - propagation si nécessaire

- Éviter :
  - logique métier dans les entités
  - accès direct au repository depuis le contrôleur

- Surveiller :
  - chargement paresseux (lazy loading)
  - problèmes de N+1

**Quand agir :**
- Bugs liés aux transactions
- Performances dégradées côté base
- Accès aux données mal contrôlé

---

## API & OpenAPI

- Documenter systématiquement avec OpenAPI :
  - `@Operation`
  - `@ApiResponse`
  - `@Parameter`

- Documenter :
  - cas nominal
  - cas d’erreur

- Ne pas exposer directement les entités JPA
- Utiliser des DTOs dédiés

- Standardiser les erreurs (ex: `ProblemDetail`)

**Quand agir :**
- API difficile à comprendre
- Contrats implicites
- Documentation absente

---

## Review Checklist

### Tests
- Tests unitaires présents pour les services
- Usage de `@SpringBootTest` justifié
- Tests par couche (Web / JPA / Service)

### Architecture
- Contrôleurs fins
- Services bien découpés
- Repositories isolés

### Code
- Injection par constructeur
- Nommage explicite
- Méthodes lisibles

### Persistence
- Transactions au bon niveau
- Pas de logique métier dans les entités
- Pas d’accès direct DB depuis les contrôleurs

### API
- OpenAPI complet
- DTO utilisés
- Erreurs documentées

### Global
- Code compréhensible rapidement
- Responsabilités claires
- Pas de complexité inutile

---

## Example (OpenAPI on Spring Boot)

```java
@GetMapping
@Operation(summary = "Récupérer tous les livres",
        description = "Récupère la liste complète des livres")
@ApiResponses({
        @ApiResponse(responseCode = "200",
                description = "Liste des livres récupérée avec succès",
                content = @Content(mediaType = "application/json",
                        array = @ArraySchema(schema = @Schema(implementation = Book.class)))),
        @ApiResponse(responseCode = "404",
                description = "Aucun livre trouvé",
                content = @Content(mediaType = "application/json",
                        schema = @Schema(implementation = ProblemDetail.class))),
        @ApiResponse(responseCode = "500",
                description = "Erreur lors du traitement",
                content = @Content(mediaType = "application/json",
                        schema = @Schema(implementation = ProblemDetail.class)))
})
public List<Book> getBooks() throws BookException {
    return bookService.findAllBooks();
}
```
