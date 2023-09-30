# JUnit Snippets

## 1. Classes with `Test` Annotation Should Have `Test` Suffix

```kotlin
@Test
fun `classes with 'Test' Annotation should have 'Test' suffix`() {
Konsist
    .scopeFromSourceSet("test") // Only look within test files
    .classes()
    .filter { it.functions().any { func -> func.hasAnnotationOf(Test::class) } } // All files with a @Test function
    .assert { it.hasNameEndingWith("Tests") }
}
```

## 2. Don\`t Use JUnit4 Test Annotation

```kotlin
@Test
fun `don't use JUnit4 Test annotation`() {
    Konsist
        .scopeFromProject()
        .classes()
        .functions()
        .assertNot { it.hasAnnotationWithName("org.junit.Test") } // should be only org.junit.jupiter.api.Test
}
```
