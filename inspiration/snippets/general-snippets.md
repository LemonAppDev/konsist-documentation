# General Snippets

## 1. F i l e s   I n   ` e x t `   P a c k a g e   M u s t   H a v e   N a m e   E n d i n g   W i t h   ` E x t `

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

## 2. P r o p e r t i e s   A r e   D e c l a r e d   B e f o r e   F u n c t i o n s

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

## 3. E v e r y   C o n s t r u c t o r   P a r a m e t e r   H a s   N a m e   D e r i v e d   F r o m   P a r a m e t e r   T y p e

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

## 4. E v e r y   C l a s s   C o n s t r u c t o r   H a s   A l p h a b e t i c a l l y   O r d e r e d   P a r a m e t e r s

```kotlin
@Test
fun `every class constructor has alphabetically ordered parameters`() {
    Konsist
        .scopeFromProject()
        .classes()
        .constructors
        .assertTrue {
            val names = it.parameters.map { parameter -> parameter.name }
            val sortedNames = names.sorted()
            names == sortedNames
        }
}
```

## 5. C o m p a n i o n   O b j e c t   I s   L a s t   D e c l a r a t i o n   I n   T h e   C l a s s

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

## 6. E v e r y   V a l u e   C l a s s   H a s   P a r a m e t e r   N a m e d   ` v a l u e `

```kotlin
@Test
fun `every value class has parameter named 'value'`() {
    Konsist
        .scopeFromProject()
        .classes()
        .withValueModifier()
        .primaryConstructors
        .assertTrue { it.hasParameterNamed("value") }
}
```

## 7. N o   E m p t y   F i l e s   A l l o w e d

```kotlin
@Test
fun `no empty files allowed`() {
    Konsist
        .scopeFromProject()
        .files
        .assertFalse { it.text.isEmpty() }
}
```

## 8. N o   F i e l d   S h o u l d   H a v e   ` m `   P r e f i x

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

## 9. N o   C l a s s   S h o u l d   U s e   F i e l d   I n j e c t i o n

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

## 10. N o   C l a s s   S h o u l d   U s e   J a v a   U t i l   L o g g i n g

```kotlin
@Test
fun `no class should use Java util logging`() {
    Konsist
        .scopeFromProject()
        .files
        .assertFalse { it.hasImport { import -> import.name == "java.util.logging.." } }
}
```

## 11. P a c k a g e   N a m e   M u s t   M a t c h   F i l e   P a t h

```kotlin
@Test
fun `package name must match file path`() {
    Konsist
        .scopeFromProject()
        .packages
        .assertTrue { it.hasMatchingPath }
}
```

## 12. N o   W i l d c a r d   I m p o r t s   A l l o w e d

```kotlin
@Test
fun `no wildcard imports allowed`() {
    Konsist
        .scopeFromProject()
        .imports
        .assertFalse { it.isWildcard }
}
```

## 13. F o r b i d   T h e   U s a g e   O f   ` f o r b i d d e n S t r i n g `   I n   F i l e

```kotlin
@Test
fun `forbid the usage of 'forbiddenString' in file`() {
    Konsist
        .scopeFromProject()
        .files
        .assertFalse { it.text.contains("forbiddenString") }
}
```

