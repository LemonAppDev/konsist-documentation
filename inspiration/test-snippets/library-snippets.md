# Library Snippets

## Snippet 1

```kotlin
fun `every api declaration has KDoc`() {
    Konsist.scopeFromPackage("..api..")
        .declarationsOf<KoKDocProvider>(includeNested = true)
        .assert { it.hasKDoc }
}
```

## Snippet 2

```kotlin
fun `every public function in api package must have explicit return type`() {
    Konsist.scopeFromPackage("..api..")
        .functions(includeNested = true)
        .assert { it.hasExplicitReturnType }
}
```

## Snippet 3

```kotlin
fun `every public property in api package must have specify type explicitly`() {
    Konsist.scopeFromPackage("..api..")
        .properties(includeNested = true)
        .assert { it.hasExplicitType() }
}
```
