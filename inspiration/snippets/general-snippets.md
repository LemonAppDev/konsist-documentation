# General Snippets

## Snippet 1

```kotlin
fun `no empty files allowed`() {
    Konsist.scopeFromProject()
        .files()
        .assertNot { it.text.isEmpty() }
}
```

## Snippet 2

```kotlin
fun `no field should have 'm' prefix`() {
    Konsist.scopeFromProject()
        .classes()
        .assert {
            val secondCharacterIsUppercase = it.name.getOrNull(1)?.isUpperCase() ?: false
            it.name.startsWith('m') && secondCharacterIsUppercase
        }
}
```

## Snippet 3

```kotlin
fun `no class should use field injection`() {
    Konsist.scopeFromProject()
        .classes()
        .assert { it.hasAnnotationOf<Inject>() }
}
```

## Snippet 4

```kotlin
fun `no class should use Java util logging`() {
    Konsist.scopeFromProject()
        .files()
        .assert { it.hasImports("java.util.logging..") }
}
```

## Snippet 5

```kotlin
fun `every constructor parameter has name derived from parameter type`() {
    Konsist.scopeFromProject()
        .classes()
        .flatMap { it.allConstructors }
        .flatMap { it.parameters }
        .assert {
            val nameTitleCase = it.name.replaceFirstChar { char -> char.titlecase(Locale.getDefault()) }
            nameTitleCase == it.type.sourceType
        }
}
```

## Snippet 6

```kotlin
fun `every class constructor has alphabetically ordered parameters`() {
    Konsist.scopeFromProject()
        .classes()
        .flatMap { it.allConstructors }
        .assert {
            val names = it.parameters.map { parameter -> parameter.name }
            val sortedNames = names.sorted()
            names == sortedNames
        }
}
```

## Snippet 7

```kotlin
fun `package name must match file path`() {
    Konsist.scopeFromProject()
        .packages()
        .assert {
            it
                .filePath
                .replace("/", ".")
                .endsWith(it.qualifiedName)
        }
}
```

## Snippet 8

```kotlin
fun `properties are declared before functions`() {
    Konsist.scopeFromProject()
        .classes()
        .assert {
            val lastKoPropertyDeclarationIndex = it
                .declarations()
                .indexOfLastInstance<KoPropertyDeclaration>()

            val firstKoFunctionDeclarationIndex = it
                .declarations()
                .indexOfFirstInstance<KoFunctionDeclaration>()

            lastKoPropertyDeclarationIndex < firstKoFunctionDeclarationIndex
        }
}
```

## Snippet 9

```kotlin
fun `companion object is the last declaration in the class`() {
    Konsist.scopeFromProject()
        .classes()
        .assert {
            val companionObjectIndex = it
                .declarations()
                .indexOfLast { declaration ->
                    declaration is KoObjectDeclaration && declaration.hasModifiers(KoModifier.COMPANION)
                }

            val lastIndex = it
                .declarations()
                .indexOfLastInstance<KoNamedDeclaration>()

            companionObjectIndex == lastIndex
        }
}
```

## Snippet 10

```kotlin
fun `no wildcard imports allowed`() {
    Konsist.scopeFromProject()
        .imports()
        .assertNot { it.isWildcard }
}
```

## Snippet 11

```kotlin
fun `every value class has parameter named 'value'`() {
    Konsist.scopeFromProject()
        .classes()
        .withValueModifier()
        .mapNotNull { it.primaryConstructor }
        .assert { it.hasParameterNamed("value") }
}
```

## Snippet 12

```kotlin
fun `every class in the 'feature' module reside in package 'feature'`() {
    Konsist.scopeFromModule("feature")
        .classes(includeNested = true)
        .assert { it.resideInPackage("..feature..") }
}
```

## Snippet 13

```kotlin
fun `forbid the usage of 'forbiddenString' in file`() {
    Konsist.scopeFromProject()
        .files()
        .assertNot { it.text.contains("forbiddenString") }
}
```
