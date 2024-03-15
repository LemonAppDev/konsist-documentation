# JUnit Snippets

Code snippets employed to ensure the uniformity of tests written with [JUnit](https://junit.org/junit5/) 
library.

## 1. Classes With `Test` Annotation Should Have `Test` Suffix

```kotlin
@Test
fun `classes with 'Test' Annotation should have 'Test' suffix`() {
    Konsist
        .scopeFromSourceSet("test")
        .classes()
        .filter {
            it.functions().any { func -> func.hasAnnotationOf(Test::class) }
        }
        .assertTrue { it.hasNameEndingWith("Tests") }
}
```

## 2. Test Classes Should Have Test Subject Named Sut

```kotlin
@Test
fun `test classes should have test subject named sut`() {
    Konsist
        .scopeFromTest()
        .classes()
        .assertTrue {
            // Get type name from test class e.g. FooTest -> Foo
            val type = it.name.removeSuffix("Test")
            val sut = it
                .properties()
                .firstOrNull { property -> property.name == "sut" }

            sut != null && sut.hasTacitType(type)
        }
}
```

## 3. Test Classes Should Have All Members Private Besides Tests

```kotlin
@Test
fun `test classes should have all members private besides tests`() {
    Konsist
        .scopeFromTest()
        .classes()
        .declarations()
        .filterIsInstance<KoAnnotationProvider>()
        .withoutAnnotationOf(Test::class, ParameterizedTest::class, RepeatedTest::class)
        .filterIsInstance<KoVisibilityModifierProvider>()
        .assertTrue { it.hasPrivateModifier }
}
```

## 4. No Class Should Use JUnit4 Test Annotation

```kotlin
@Test
fun `no class should use JUnit4 Test annotation`() {
    Konsist
        .scopeFromProject()
        .classes()
        .functions()
        .assertFalse {
            it.annotations.any { annotation ->
                annotation.fullyQualifiedName == "org.junit.Test"
            }
        }
}
```

