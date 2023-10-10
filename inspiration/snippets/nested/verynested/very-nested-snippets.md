# Veeeeery Nested Snippets

Veeeery nested example

## 1. Veeeeery Nested Test

```kotlin
@Test
fun `veeeeery nested test`() {
    Konsist
        .scopeFromProject()
        .classes()
        .withParentOf(ViewModel::class)
        .assertTrue { it.name.endsWith("ViewModel") }
}
```

