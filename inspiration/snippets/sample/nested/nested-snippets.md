# Nested Snippets

Neeeeested snippets

## 1. Nested Test

```kotlin
@Test
fun `nested test`() {
    Konsist
        .scopeFromProject()
        .classes()
        .withParentOf(ViewModel::class)
        .assertTrue { it.name.endsWith("ViewModel") }
}
```

