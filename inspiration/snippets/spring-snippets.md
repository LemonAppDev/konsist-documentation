# Spring Snippets

Konsist can be used to guard the consistency of the [Spring](https://spring.io/) project.

## Snippet 1: Interfaces With `Repository` Annotation Should Have `Repository` Suffix

```kotlin
@Test
fun `interfaces with 'Repository' annotation should have 'Repository' suffix`() {
    Konsist
        .scopeFromProject()
        .interfaces()
        .withAllAnnotationsOf(Repository::class)
        .assert { it.hasNameEndingWith("Repository") }
}
```

## Snippet 2: Classes With `RestController` Annotation Should Have `Controller` Suffix

```kotlin
@Test
fun `classes with 'RestController' annotation should have 'Controller' suffix`() {
    Konsist
        .scopeFromProject()
        .classes()
        .withAllAnnotationsOf(RestController::class)
        .assert { it.hasNameEndingWith("Controller") }
}
```

## Snippet 3: Classes With `RestController` Annotation Should Reside In `controller` Package

```kotlin
@Test
fun `classes with 'RestController' annotation should reside in 'controller' package`() {
    Konsist
        .scopeFromProject()
        .classes()
        .withAllAnnotationsOf(RestController::class)
        .assert { it.resideInPackage("..controller..") }
}
```

