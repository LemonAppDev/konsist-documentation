# Spring Snippets

Konsist can be used to guard the consistency of the [Spring](https://spring.io/) project.

## 1. I n t e r f a c e s   W i t h   ` R e p o s i t o r y `   A n n o t a t i o n   S h o u l d   H a v e   ` R e p o s i t o r y `   S u f f i x

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

## 2. C l a s s e s   W i t h   ` R e s t C o n t r o l l e r `   A n n o t a t i o n   S h o u l d   H a v e   ` C o n t r o l l e r `   S u f f i x

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

## 3. C l a s s e s   W i t h   ` R e s t C o n t r o l l e r `   A n n o t a t i o n   S h o u l d   R e s i d e   I n   ` c o n t r o l l e r `   P a c k a g e

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

