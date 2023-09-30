# JUnit Snippets

## 1. Classes with `Test` Annotation Should Have `Test` Suffix

```kotlin
@Test
fun `classes with 'Test' Annotation should have 'Test' suffix`() {
Konsist
    .scopeFromSourceSet("test")
    .classes()
    .filter { it.functions().any { func -> func.hasAnnotationOf(Test::class) } }
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
        .assertNot { it.hasAnnotationWithName("org.junit.Test") }
}
```
