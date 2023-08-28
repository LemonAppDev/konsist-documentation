# Test Snippets
## Snippet 1: Every Class Has Test

```kotlin
@Test
fun `every class has test`() {
    Konsist
        .scopeFromProduction()
        .classes()
        .assert { it.hasTest() }
}
```

## Snippet 2: Every Class - Except Data And Value Class - Has Test

```kotlin
@Test
fun `every class - except data and value class - has test`() {
    Konsist
        .scopeFromProduction()
        .classes()
        .withoutSomeModifiers(KoModifier.DATA, KoModifier.VALUE)
        .assert { it.hasTest() }
}
```

## Snippet 3: Test Classes Should Have Test Subject Named Sut

```kotlin
@Test
fun `test classes should have test subject named sut`() {
    Konsist
        .scopeFromTest()
        .classes()
        .assert {
            val type = it.name.removeSuffix("Test")
            val sut = it
                .properties()
                .firstOrNull { property -> property.name == "sut" }

            sut != null && (sut.type?.name == type || sut.text.contains("$type("))
        }
}
```

## Snippet 4: Test Classes Should Have All Members Private Besides Tests

```kotlin
@Test
fun `test classes should have all members private besides tests`() {
    Konsist
        .scopeFromTest()
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

## Snippet 5: Don't Use JUnit4 Test Annotation

```kotlin
@Test
fun `don't use JUnit4 Test annotation`() {
    Konsist
        .scopeFromProject()
        .classes()
        .functions()
        .assertNot { it.hasAnnotations("org.junit.Test") } // should be only org.junit.jupiter.api.Test
}
```

