# Library Snippets
## Snippet 5: Every Api Declaration Has KDoc

```kotlin
@Test
fun `every api declaration has KDoc`() {
    Konsist
        .scopeFromPackage("..api..")
        .declarationsOf<KoKDocProvider>(includeNested = true)
        .assert { it.hasKDoc }
}
```

## Snippet 5: Every Public Function In Api Package Must Have Explicit Return Type

```kotlin
@Test
fun `every public function in api package must have explicit return type`() {
    Konsist
        .scopeFromPackage("..api..")
        .functions(includeNested = true)
        .assert { it.hasReturnType }
}
```

## Snippet 5: Every Public Property In Api Package Must Have Specify Type Explicitly

```kotlin
@Test
fun `every public property in api package must have specify type explicitly`() {
    Konsist
        .scopeFromPackage("..api..")
        .properties(includeNested = true)
        .assert { it.hasType() }
}
```

