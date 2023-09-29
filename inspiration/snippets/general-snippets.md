# General Snippets

## 1. Files In `ext` Package Must Have Name Ending With `Ext`

```kotlin
@Test
fun `files in 'ext' package must have name ending with 'Ext'`() {
    Konsist
        .scopeFromProject()
        .files
        .withPackage("..ext..")
        .assert { it.hasNameEndingWith("Ext") }
}
```

## 2. Properties Are Declared Before Functions

```kotlin
@Test
fun `properties are declared before functions`() {
    Konsist
        .scopeFromProject()
        .classes()
        .assert {
            val lastKoPropertyDeclarationIndex = it
                .declarations(includeNested = false, includeLocal = false)
                .indexOfLastInstance<KoPropertyDeclaration>()

            val firstKoFunctionDeclarationIndex = it
                .declarations(includeNested = false, includeLocal = false)
                .indexOfFirstInstance<KoFunctionDeclaration>()

            if (lastKoPropertyDeclarationIndex != -1 && firstKoFunctionDeclarationIndex != -1) {
                lastKoPropertyDeclarationIndex < firstKoFunctionDeclarationIndex
            } else {
                true
            }
        }
}
```

## 3. Every Constructor Parameter Has Name Derived From Parameter Type

```kotlin
@Test
fun `every constructor parameter has name derived from parameter type`() {
    Konsist
        .scopeFromProject()
        .classes()
        .constructors
        .parameters
        .assert {
            val nameTitleCase = it.name.replaceFirstChar { char -> char.titlecase(Locale.getDefault()) }
            nameTitleCase == it.type.sourceType
        }
}
```

## 4. Every Class Constructor Has Alphabetically Ordered Parameters

```kotlin
@Test
fun `every class constructor has alphabetically ordered parameters`() {
    Konsist
        .scopeFromProject()
        .classes()
        .constructors
        .assert {
            val names = it.parameters.map { parameter -> parameter.name }
            val sortedNames = names.sorted()
            names == sortedNames
        }
}
```

## 5. Companion Object Is Last Declaration In The Class

```kotlin
@Test
fun `companion object is last declaration in the class`() {
    Konsist
        .scopeFromProject()
        .classes()
        .assert {
            val companionObject = it.objects(includeNested = false).lastOrNull { obj ->
                obj.hasModifier(KoModifier.COMPANION)
            }

            if (companionObject != null) {
                it.declarations(includeNested = false, includeLocal = false).last() == companionObject
            } else {
                true
            }
        }
}
```

## 6. Every Value Class Has Parameter Named `value`

```kotlin
@Test
fun `every value class has parameter named 'value'`() {
    Konsist
        .scopeFromProject()
        .classes()
        .withValueModifier()
        .primaryConstructors
        .assert { it.hasParameterWithName("value") }
}
```

## 7. No Empty Files Allowed

```kotlin
@Test
fun `no empty files allowed`() {
    Konsist
        .scopeFromProject()
        .files
        .assertNot { it.text.isEmpty() }
}
```

## 8. No Field Should Have `m` Prefix

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

## 9. No Class Should Use Field Injection

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

## 10. No Class Should Use Java Util Logging

```kotlin
@Test
fun `no class should use Java util logging`() {
    Konsist
        .scopeFromProject()
        .files
        .assertNot { it.hasImport { import -> import.name == "java.util.logging.." } }
}
```

## 11. Package Name Must Match File Path

```kotlin
@Test
fun `package name must match file path`() {
    Konsist
        .scopeFromProject()
        .packages
        .assert { it.hasMatchingPath }
}
```

## 12. No Wildcard Imports Allowed

```kotlin
@Test
fun `no wildcard imports allowed`() {
    Konsist
        .scopeFromProject()
        .imports
        .assertNot { it.isWildcard }
}
```

## 13. Forbid The Usage Of `forbiddenString` In File

```kotlin
@Test
fun `forbid the usage of 'forbiddenString' in file`() {
    Konsist
        .scopeFromProject()
        .files
        .assertNot { it.text.contains("forbiddenString") }
}
```

## 14. All files with JUnit @Test annotations end with the name "Tests"

```kotlin
@Test
fun `all files with tests in them should end with 'Tests'`() {
Konsist
    .scopeFromSourceSet("test") // Only look within test files
    .files
    .filter { it.functions().any { fn -> fn.hasAnnotationOf(Test::class) } } // All files with a @Test function
    .assert { it.hasNameEndingWith("Tests") }
}
```