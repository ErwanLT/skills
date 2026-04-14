# Quarkus Best Practices (Tests, Patterns)

## Table of Contents
- Testing
- Architecture & Layers
- Common Patterns
- API & OpenAPI
- Review Checklist

---

## Testing

- Utiliser `@QuarkusTest` pour les tests d’intégration.
- Limiter son usage : démarrage du contexte = coût non négligeable.
- Écrire des **tests unitaires purs** sans Quarkus dès que possible.
- Utiliser `@QuarkusTestResource` pour gérer les dépendances externes (DB, services).
- Séparer clairement :
  - tests rapides (unitaires)
  - tests avec contexte (intégration)

- Structurer les tests en **Given / When / Then**
- Tester le comportement, pas l’implémentation

**Quand agir :**
- Tests trop lents
- Couverture faible hors intégration
- Dépendance excessive au runtime Quarkus

---

## Architecture & Layers

- Séparer clairement :
  - **Resource (API)** → exposition HTTP
  - **Service** → logique métier
  - **Repository** → accès aux données

- Les endpoints doivent rester **fins**
- Toute logique métier doit être dans les services
- Ne pas faire remonter de logique technique dans le domaine

- Favoriser des services **cohérents et ciblés**
- Éviter les classes “fourre-tout”

**Quand agir :**
- Logique métier dans les ressources
- Services trop volumineux ou ambigus
- Couplage fort entre couches

---

## Common Patterns

- Injection via `@Inject`
  - Autoriser les constructeurs pour améliorer la testabilité

- Scopes :
  - `@ApplicationScoped` pour les services
  - `@Singleton` si stateless pur

- Utiliser des **DTOs** pour l’exposition externe
- Mapper explicitement DTO ↔ domaine

- Préférer la composition à l’héritage

- Patterns fréquents :
  - **Strategy** : variantes métier
  - **Factory** : instanciation complexe
  - **Adapter** : intégration externe

**Quand agir :**
- Duplication de logique
- Couplage fort avec des API externes
- Difficulté à faire évoluer le code

---

## API & OpenAPI

- Documenter systématiquement avec OpenAPI :
  - `@Operation`
  - `@APIResponse`
  - `@Parameter`

- Toujours documenter :
  - cas nominal
  - erreurs possibles

- Utiliser des objets dédiés pour les réponses (DTO)
- Ne jamais exposer directement les entités internes

- Standardiser les erreurs (ex: `ProblemDetail`)

**Quand agir :**
- API difficile à comprendre
- Contrats implicites
- Documentation absente ou incomplète

---

## Review Checklist

### Tests
- Tests unitaires présents sans démarrage Quarkus
- Usage de `@QuarkusTest` justifié
- Cas limites et erreurs couverts

### Architecture
- Resource fine, sans logique métier
- Services bien découpés
- Accès aux données isolé

### Code
- Nommage explicite
- Méthodes courtes et lisibles
- Pas de duplication évidente

### API
- OpenAPI complet
- DTO utilisés
- Erreurs documentées

### Global
- Code compréhensible rapidement
- Responsabilités claires
- Pas de complexité inutile

---

## Example (OpenAPI on Quarkus)

```java
@GET
@Path("/greeting")
@Operation(summary = "Saluer un aventurier",
        description = "Retourne un message d'accueil de l'aubergiste.")
@APIResponse(
        responseCode = "200",
        description = "Message d'accueil",
        content = @Content(schema = @Schema(implementation = TavernGreetingResponse.class))
)
@APIResponse(
        responseCode = "400",
        description = "Paramètre invalide",
        content = @Content(schema = @Schema(implementation = ProblemDetail.class))
)
public TavernGreetingResponse greeting(
        @Parameter(description = "Nom de l'aventurier à saluer", example = "Arthas")
        @QueryParam("name") String name
) {
    return tavernService.greeting(name);
}
```