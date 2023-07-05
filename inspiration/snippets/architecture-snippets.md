# Architecture Snippets

## Snippet 1

```kotlin
fun `2 layer architecture has correct dependencies`() {
    Konsist
        .scopeFromProject()
        .assertArchitecture {
            val presentation = Layer("Presentation", "com.myapp.presentation..")
            val data = Layer("Data", "com.myapp.data..")

            presentation.dependsOn(data)
            data.dependsOnNothing()
        }
}
```

