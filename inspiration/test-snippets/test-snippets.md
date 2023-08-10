# Test Snippets

## Snippet 1

```kotlin
fun `every class has test`() {
    Konsist.scopeFromProduction()
        .classes()
        .assert { it.hasTest() }
}
```

## Snippet 2

```kotlin
fun `every class - except data and value class - has test`() {
    Konsist.scopeFromProduction()
        .classes()
        .withoutSomeModifiers(KoModifier.DATA, KoModifier.VALUE)
        .assert { it.hasTest() }
}
```

## Snippet 3

```kotlin
fun `test classes should have test subject named sut`() {
    Konsist.scopeFromTest()
        .classes()
        .assert {
            val type = it.name.removeSuffix("Test")
            val sut = it
                .properties()
                .firstOrNull { property -> property.name == "sut" }

            sut != null && (sut.explicitType?.name == type || sut.text.contains("$type("))
        }
}
```

## Snippet 4

```kotlin
fun `test classes should have all members private besides tests`() {
    Konsist.scopeFromTest()
        .classes()
        .declarations()
        .filterIsInstance<KoAnnotationProvider>()
        .filterNot {
            it.annotations.any { annotation ->
                annotation
                    .name
                    .lowercase()
                    .contains("test")
            }
        }
        .filterIsInstance<KoVisibilityModifierProvider>()
        .assert { it.hasPrivateModifier }
}
```

## Snippet 5

```kotlin
fun `don't use JUnit4 Test annotation`() {
    Konsist.scopeFromProject()
        .classes()
        .functions()
        .assertNot { it.hasAnnotations("org.junit.Test") } // should be only org.junit.jupiter.api.Test
}
```
