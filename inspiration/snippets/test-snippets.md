# Test Snippets

## 1. E v e r y   C l a s s   H a s   T e s t

```kotlin
@Test
fun `every class has test`() {
    Konsist
        .scopeFromProduction()
        .classes()
        .assertTrue { it.hasTestClass() }
}
```

## 2. E v e r y   C l a s s   -   E x c e p t   D a t a   A n d   V a l u e   C l a s s   -   H a s   T e s t

```kotlin
@Test
fun `every class - except data and value class - has test`() {
    Konsist
        .scopeFromProduction()
        .classes()
        .withoutModifier(KoModifier.DATA, KoModifier.VALUE)
        .assertTrue { it.hasTestClass() }
}
```

## 3. T e s t   C l a s s e s   S h o u l d   H a v e   T e s t   S u b j e c t   N a m e d   S u t

```kotlin
@Test
fun `test classes should have test subject named sut`() {
    Konsist
        .scopeFromTest()
        .classes()
        .assertTrue {
            val type = it.name.removeSuffix("Test")
            val sut = it
                .properties()
                .firstOrNull { property -> property.name == "sut" }

            sut != null && (sut.type?.name == type || sut.text.contains("$type("))
        }
}
```

## 4. T e s t   C l a s s e s   S h o u l d   H a v e   A l l   M e m b e r s   P r i v a t e   B e s i d e s   T e s t s

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

## 5. D o n ` t   U s e   J U n i t 4   T e s t   A n n o t a t i o n

```kotlin
@Test
fun `don't use JUnit4 Test annotation`() {
    Konsist
        .scopeFromProject()
        .classes()
        .functions()
        .assertFalse { it.hasAnnotationWithName("org.junit.Test") } // should be only org.junit.jupiter.api.Test
}
```

