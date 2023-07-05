# Spring Snippets

Konsist can be used to guard the consistency of the [Spring](https://spring.io/) project.

## Snippet 1

```kotlin
fun `classes with 'Repository' annotation should have 'Repository' suffix`() {
    Konsist
        .scopeFromProject()
        .classes()
        .withAnnotationOf<Repository>()
        .assert { it.hasNameEndingWith("Repository") }
}
```

## Snippet 2

```kotlin
fun `classes with 'RestController' annotation should have 'Controller' suffix`() {
    Konsist
        .scopeFromProject()
        .classes()
        .withAnnotationOf<RestController>()
        .assert { it.hasNameEndingWith("Controller") }
}
```

## Snippet 3

```kotlin
fun `classes with 'RestController' annotation should reside in 'controller' package`() {
    Konsist
        .scopeFromProject()
        .classes()
        .withAnnotationOf<RestController>()
        .assert { it.resideInPackage("..controller..") }
}
```

##
