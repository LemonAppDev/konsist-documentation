# General Snippets
## Snippet 1: No Empty Files Allowed

```kotlin
@Test
fun `no empty files allowed`() {
    Konsist
        .scopeFromProject()
        .files
        .assertNot { it.text.isEmpty() }
}
```

## Snippet 2: No Field Should Have 'm' Prefix

```kotlin
@Test
fun `no field should have 'm' prefix`() {
    Konsist
        .scopeFromProject()
        .classes()
        .properties()
        .assertNot {
            val secondCharacterIsUppercase = it.name.getOrNull(1)?.isUpperCase() ?: false
            it.name.startsWith('m') && secondCharacterIsUppercase
        }
}
```

## Snippet 3: No Class Should Use Field Injection

```kotlin
@Test
fun `no class should use field injection`() {
    Konsist
        .scopeFromProject()
        .classes()
        .properties()
        .assertNot { it.hasAnnotationOf<Inject>() }
}
```

## Snippet 4: No Class Should Use Java Util Logging

```kotlin
@Test
fun `no class should use Java util logging`() {
    Konsist
        .scopeFromProject()
        .files
        .assertNot { it.hasImports("java.util.logging..") }
}
```

## Snippet 5: Every Constructor Parameter Has Name Derived From Parameter Type

```kotlin
@Test
fun `every constructor parameter has name derived from parameter type`() {
    Konsist
        .scopeFromProject()
        .classes()
        .flatMap { it.constructors }
        .flatMap { it.parameters }
        .assert {
            val nameTitleCase = it.name.replaceFirstChar { char -> char.titlecase(Locale.getDefault()) }
            nameTitleCase == it.type.sourceType
        }
}
```

## Snippet 6: Every Class Constructor Has Alphabetically Ordered Parameters

```kotlin
@Test
fun `every class constructor has alphabetically ordered parameters`() {
    Konsist
        .scopeFromProject()
        .classes()
        .flatMap { it.constructors }
        .assert {
            val names = it.parameters.map { parameter -> parameter.name }
            val sortedNames = names.sorted()
            names == sortedNames
        }
}
```

## Snippet 7: Package Name Must Match File Path

```kotlin
@Test
fun `package name must match file path`() {
    Konsist
        .scopeFromProject()
        .packages
        .assert { it.hasMatchingPath }
}
```

## Snippet 8: Properties Are Declared Before Functions

```kotlin
@Test
fun `properties are declared before functions`() {
    Konsist
        .scopeFromProject()
        .classes()
        .assert {
            val lastKoPropertyDeclarationIndex = it
                .declarations()
                .indexOfLastInstance<KoPropertyDeclaration>()

            val firstKoFunctionDeclarationIndex = it
                .declarations()
                .indexOfFirstInstance<KoFunctionDeclaration>()

            lastKoPropertyDeclarationIndex <= firstKoFunctionDeclarationIndex
        }
}
```

## Snippet 9: Companion Object Is The Last Declaration In The Class

```kotlin
@Test
fun `companion object is the last declaration in the class`() {
    Konsist
        .scopeFromProject()
        .classes()
        .assert {
            val companionObjectIndex = it
                .declarations()
                .indexOfLast { declaration ->
                    declaration is KoObjectDeclaration && declaration.hasModifiers(KoModifier.COMPANION)
                }

            val lastIndex = it.numDeclarations() - 1

            companionObjectIndex == lastIndex || companionObjectIndex == -1
        }
}
```

## Snippet 10: No Wildcard Imports Allowed

```kotlin
@Test
fun `no wildcard imports allowed`() {
    Konsist
        .scopeFromProject()
        .imports
        .assertNot { it.isWildcard }
}
```

## Snippet 11: Every Value Class Has Parameter Named 'value'

```kotlin
@Test
fun `every value class has parameter named 'value'`() {
    Konsist
        .scopeFromProject()
        .classes()
        .withValueModifier()
        .mapNotNull { it.primaryConstructor }
        .assert { it.hasParameterNamed("value") }
}
```

## Snippet 12: Forbid The Usage Of 'forbiddenString' In File

```kotlin
@Test
fun `forbid the usage of 'forbiddenString' in file`() {
    Konsist
        .scopeFromProject()
        .files
        .assertNot { it.text.contains("forbiddenString") }
}
```

