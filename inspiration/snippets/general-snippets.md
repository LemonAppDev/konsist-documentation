# General Snippets

## 1. Files In `ext` Package Must Have Name Ending With `Ext`

```kotlin
@Test
fun `files in 'ext' package must have name ending with 'Ext'`() {
    Konsist
        .scopeFromProject()
        .files
        .withPackage("..ext..")
        .assertTrue { it.hasNameEndingWith("Ext") }
}
```

## 2. All Data Class Properties Are Defined In Constructor

```kotlin
@Test
fun `all data class properties are defined in constructor`() {
    Konsist
        .scopeFromProject()
        .classes()
        .withModifier(KoModifier.DATA)
        .properties()
        .assertTrue { it.isConstructorDefined }
}
```

## 3. Every Class Has Test

```kotlin
@Test
fun `every class has test`() {
    Konsist
        .scopeFromProduction()
        .classes()
        .assertTrue { it.hasTestClasses() }
}
```

## 4. Every Class - Except Data And Value Class - Has Test

```kotlin
@Test
fun `every class - except data and value class - has test`() {
    Konsist
        .scopeFromProduction()
        .classes()
        .withoutModifier(KoModifier.DATA, KoModifier.VALUE)
        .assertTrue { it.hasTestClasses() }
}
```

## 5. Properties Are Declared Before Functions

```kotlin
@Test
fun `properties are declared before functions`() {
    Konsist
        .scopeFromProject()
        .classes()
        .assertTrue {
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

## 6. Every Constructor Parameter Has Name Derived From Parameter Type

```kotlin
@Test
fun `every constructor parameter has name derived from parameter type`() {
    Konsist
        .scopeFromProject()
        .classes()
        .constructors
        .parameters
        .assertTrue {
            val nameTitleCase = it.name.replaceFirstChar { char -> char.titlecase(Locale.getDefault()) }
            nameTitleCase == it.type.sourceType
        }
}
```

## 7. Every Class Constructor Has Alphabetically Ordered Parameters

```kotlin
@Test
fun `every class constructor has alphabetically ordered parameters`() {
    Konsist
        .scopeFromProject()
        .classes()
        .constructors
        .assertTrue { it.parameters.isSortedByName() }
}
```

## 8. Enums Has Alphabetically Ordered Consts

```kotlin
@Test
fun `enums has alphabetically ordered consts`() {
    Konsist
        .scopeFromProduction()
        .classes()
        .withAllModifiers(KoModifier.ENUM)
        .assertTrue { it.enumConstants.isSortedByName() }
}
```

## 9. Companion Object Is Last Declaration In The Class

```kotlin
@Test
fun `companion object is last declaration in the class`() {
    Konsist
        .scopeFromProject()
        .classes()
        .assertTrue {
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

## 10. Every Value Class Has Parameter Named `value`

```kotlin
@Test
fun `every value class has parameter named 'value'`() {
    Konsist
        .scopeFromProject()
        .classes()
        .withValueModifier()
        .primaryConstructors
        .assertTrue { it.hasParameterWithName("value") }
}
```

## 11. No Empty Files Allowed

```kotlin
@Test
fun `no empty files allowed`() {
    Konsist
        .scopeFromProject()
        .files
        .assertFalse { it.text.isEmpty() }
}
```

## 12. No Field Should Have `m` Prefix

```kotlin
@Test
fun `no field should have 'm' prefix`() {
    Konsist
        .scopeFromProject()
        .classes()
        .properties()
        .assertFalse {
            val secondCharacterIsUppercase = it.name.getOrNull(1)?.isUpperCase() ?: false
            it.name.startsWith('m') && secondCharacterIsUppercase
        }
}
```

## 13. No Class Should Use Field Injection

```kotlin
@Test
fun `no class should use field injection`() {
    Konsist
        .scopeFromProject()
        .classes()
        .properties()
        .assertFalse { it.hasAnnotationOf<Inject>() }
}
```

## 14. No Class Should Use Java Util Logging

```kotlin
@Test
fun `no class should use Java util logging`() {
    Konsist
        .scopeFromProject()
        .files
        .assertFalse { it.hasImport { import -> import.name == "java.util.logging.." } }
}
```

## 15. Package Name Must Match File Path

```kotlin
@Test
fun `package name must match file path`() {
    Konsist
        .scopeFromProject()
        .packages
        .assertTrue { it.hasMatchingPath }
}
```

## 16. No Wildcard Imports Allowed

```kotlin
@Test
fun `no wildcard imports allowed`() {
    Konsist
        .scopeFromProject()
        .imports
        .assertFalse { it.isWildcard }
}
```

## 17. Forbid The Usage Of `forbiddenString` In File

```kotlin
@Test
fun `forbid the usage of 'forbiddenString' in file`() {
    Konsist
        .scopeFromProject()
        .files
        .assertFalse { it.hasTextContaining("forbiddenString") }
}
```

## 18. All Function Parameters Are Interfaces

```kotlin
@Test
fun `all function parameters are interfaces`() {
    Konsist
        .scopeFromProject()
        .functions()
        .parameters
        .types
        .assertTrue { it.sourceDeclaration?.isInterface }
}
```

## 19. All Parent Interfaces Are Public

```kotlin
@Test
fun `all parent interfaces are public`() {
    Konsist
        .scopeFromProject()
        .classes()
        .parentInterfaces()
        .sourceDeclarations()
        .interfaceDeclarations()
        .assertTrue { it.hasPublicModifier }
}
```

## 20. Return Type Of All Functions Are Immutable

```kotlin
@Test
fun `return type of all functions are immutable`() {
    Konsist
        .scopeFromProject()
        .functions()
        .returnTypes
        .assertFalse { it.isMutableType }
}
```

