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

## Snippet 2

```kotlin
fun `every file in module reside in module specific package`() {
    Konsist.scopeFromProject()
        .files
        .assert { it.packagee?.fullyQualifiedName?.startsWith(it.moduleName) }
}
```
