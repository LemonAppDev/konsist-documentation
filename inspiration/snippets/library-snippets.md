# Library Snippets

## Snippet 1

```kotlin
fun `every api declaration has KDoc`() {
    Konsist.scopeFromPackage("..api..")
        .declarations(includeNested = true)
        .assert { it.hasKDoc() }
}
```

## Snippet 2

```kotlin
fun `every api declaration has complete KDoc`() {
    Konsist.scopeFromPackage("..api..")
        .declarations(includeNested = true)
        .assert { it.hasValidKDoc() }
}
```

## Snippet 3

```kotlin
fun `every public function in api package must have explicit return type`() {
    Konsist.scopeFromPackage("..api..")
        .functions(includeNested = true)
        .assert { it.hasReturnType() }
}
```

## Snippet 4

```kotlin
fun `every public property in api package must have specify type explicitly`() {
    Konsist.scopeFromPackage("..api..")
        .properties(includeNested = true)
        .assert { it.hasType() }
}
```
