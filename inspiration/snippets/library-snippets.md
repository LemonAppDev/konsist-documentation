# Library Snippets

## 1. Every Api Declaration Has KDoc

```kotlin
@Test
fun `every api declaration has KDoc`() {
    Konsist
        .scopeFromPackage("..api..")
        .declarationsOf<KoKDocProvider>()
        .assertTrue { it.hasKDoc }
}
```

## 2. Every Function With Parameters Has A Param Tags

```kotlin
@Test
fun `every function with parameters has a param tags`() {
    Konsist.scopeFromPackage("..api..")
        .functions()
        .assertTrue { it.hasValidKDocParamTags() }
}
```

## 3. Every Function With Return Value Has A Return Tag

```kotlin
@Test
fun `every function with return value has a return tag`() {
    Konsist.scopeFromPackage("..api..")
        .functions()
        .assertTrue { it.hasValidKDocReturnTag() }
}
```

## 4. Every Extension Has A Receiver Tag

```kotlin
@Test
fun `every extension has a receiver tag`() {
    Konsist.scopeFromPackage("..api..")
        .declarationsOf<KoReceiverTypeProvider>()
        .assertTrue { it.hasValidKDocReceiverTag() }
}
```

## 5. Every Public Function In Api Package Must Have Explicit Return Type

```kotlin
@Test
fun `every public function in api package must have explicit return type`() {
    Konsist
        .scopeFromPackage("..api..")
        .functions()
        .assertTrue { it.hasReturnType() }
}
```

## 6. Every Public Property In Api Package Must Have Specify Type Explicitly

```kotlin
@Test
fun `every public property in api package must have specify type explicitly`() {
    Konsist
        .scopeFromPackage("..api..")
        .properties()
        .assertTrue { it.hasType() }
}
```

