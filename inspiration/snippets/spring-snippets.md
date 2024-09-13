# Spring Snippets

Konsist can be used to guard the consistency of the [Spring](https://spring.io/) project.

## 1. Interfaces With `Repository` Annotation Should Have `Repository` Suffix

```kotlin
@Test
fun `interfaces with 'Repository' annotation should have 'Repository' suffix`() {
    Konsist
        .scopeFromProject()
        .interfaces()
        .withAnnotationOf(Repository::class)
        .assertTrue { it.hasNameEndingWith("Repository") }
}
```

## 2. Classes With `RestController` Annotation Should Have `Controller` Suffix

```kotlin
@Test
fun `classes with 'RestController' annotation should have 'Controller' suffix`() {
    Konsist
        .scopeFromProject()
        .classes()
        .withAnnotationOf(RestController::class)
        .assertTrue { it.hasNameEndingWith("Controller") }
}
```

## 3. Controllers Never Returns Collection Types

```kotlin
@Test
fun `controllers never returns collection types`() {
    /*
    Avoid returning collection types directly. Structuring the response as
    an object that contains a collection field is preferred. This approach
    allows for future expansion (e.g., adding more properties like "totalPages")
    without disrupting the existing API contract, which would happen if a JSON
    array were returned directly.
    */
    Konsist
        .scopeFromPackage("story.controller..")
        .classes()
        .withAnnotationOf(RestController::class)
        .functions()
        .assertFalse { function ->
            function.hasReturnType { it.isKotlinCollectionType }
        }
}
```

## 4. Classes With `RestController` Annotation Should Reside In `controller` Package

```kotlin
@Test
fun `classes with 'RestController' annotation should reside in 'controller' package`() {
    Konsist
        .scopeFromProject()
        .classes()
        .withAnnotationOf(RestController::class)
        .assertTrue { it.resideInPackage("..controller..") }
}
```

## 5. Classes With `RestController` Annotation Should Never Return Collection

```kotlin
@Test
fun `classes with 'RestController' annotation should never return collection`() {
    Konsist
        .scopeFromPackage("story.controller..")
        .classes()
        .withAnnotationOf(RestController::class)
        .functions()
        .assertFalse { function ->
            function.hasReturnType { it.hasNameStartingWith("List") }
        }
}
```

## 6. Service Classes Should Be Annotated With Service Annotation

```kotlin
@Test
fun `Service classes should be annotated with Service annotation`() {
    Konsist
        .scopeFromProject()
        .classes()
        .withNameEndingWith("Service")
        .assertTrue { it.hasAnnotationOf(Service::class) }
}
```

## 7. Entity Classes Should Have An Id Field

```kotlin
@Test
fun `Entity classes should have an Id field`() {
    Konsist
        .scopeFromProject()
        .classes()
        .withAnnotationOf(Entity::class)
        .assertTrue { clazz ->
            clazz.properties().any { property ->
                property.hasAnnotationOf(Id::class)
            }
        }
}
```

## 8. DTO Classes Should Be Data Classes

```kotlin
@Test
fun `DTO classes should be data classes`() {
    Konsist
        .scopeFromProject()
        .classes()
        .withNameEndingWith("DTO")
        .assertTrue { it.hasModifier(KoModifier.DATA) }
}
```

## 9. RestControllers Should Not Have State Fields

```kotlin
@Test
fun `RestControllers should not have state fields`() {
    Konsist
        .scopeFromProject()
        .classes()
        .withAnnotationOf(RestController::class)
        .objects()
        .withModifier(KoModifier.COMPANION)
        .assertTrue {
            it.properties().isEmpty()
        }
}
```

