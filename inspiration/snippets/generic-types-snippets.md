# Generic Types Snippets

## 1. All Generic Return Types Contain X In Their Name

```kotlin
@Test
fun `all generic return types contain X in their name`() {
    Konsist
        .scopeFromProduction()
        .functions()
        .returnTypes
        .withGeneric()
        .assertTrue { it.hasNameContaining("X") }
}
```

## 2. Property Generic Type Does Not Contains Star Projection

```kotlin
@Test
fun `property generic type does not contains star projection`() {
    Konsist
        .scopeFromProduction()
        .properties()
        .types
        .assertFalse { type ->
            type
                .typeArguments
                ?.flatten()
                ?.any { it.isStarProjection }
        }
}
```

## 3. All Generic Return Types Contain Kotlin Collection Type Argument

```kotlin
@Test
fun `all generic return types contain Kotlin collection type argument`() {
    Konsist
        .scopeFromProduction()
        .functions()
        .returnTypes
        .withGeneric()
        .typeArguments
        .assertTrue { it.sourceDeclaration?.isKotlinCollectionType }
}
```

## 4. Function Parameter Has Generic Type Argument With Name Ending With `Repository`

```kotlin
@Test
fun `function parameter has generic type argument with name ending with 'Repository'`() {
    Konsist
        .scopeFromProduction()
        .functions()
        .parameters
        .types
        .withGeneric()
        .sourceDeclarations()
        .assertFalse { it.hasNameEndingWith("Repository") }
}
```

