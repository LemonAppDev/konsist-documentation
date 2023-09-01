# Architecture Snippets

## Snippet 1: 2 Layer Architecture Has Correct Dependencies

```kotlin
@Test
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

## Snippet 2: Every File In Module Reside In Module Specific Package

```kotlin
@Test
fun `every file in module reside in module specific package`() {
    Konsist
        .scopeFromProject()
        .files
        .assert { it.packagee?.fullyQualifiedName?.startsWith(it.moduleName) }
}
```

