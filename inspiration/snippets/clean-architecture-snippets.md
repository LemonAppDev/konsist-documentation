# Clean Architecture Snippets

## 1. C l e a n   A r c h i t e c t u r e   L a y e r s   H a v e   C o r r e c t   D e p e n d e n c i e s

```kotlin
@Test
fun `clean architecture layers have correct dependencies`() {
    Konsist
        .scopeFromProduction()
        .assertArchitecture {
            // Define layers
            val domain = Layer("Domain", "com.myapp.domain..")
            val presentation = Layer("Presentation", "com.myapp.presentation..")
            val data = Layer("Data", "com.myapp.data..")

            // Define architecture assertions
            domain.dependsOnNothing()
            presentation.dependsOn(domain)
            data.dependsOn(domain)
        }
}
```

## 2. C l a s s e s   W i t h   ` U s e C a s e `   S u f f i x   S h o u l d   R e s i d e   I n   ` d o m a i n `   A n d   ` u s e c a s e `   P a c k a g e

```kotlin
@Test
fun `classes with 'UseCase' suffix should reside in 'domain' and 'usecase' package`() {
    Konsist
        .scopeFromProject()
        .classes()
        .withNameEndingWith("UseCase")
        .assertTrue { it.resideInPackage("..domain..usecase..") }
}
```

## 3. C l a s s e s   W i t h   ` U s e C a s e `   S u f f i x   S h o u l d   H a v e   S i n g l e   ` p u b l i c   O p e r a t o r `   M e t h o d   N a m e d   ` i n v o k e `

```kotlin
@Test
fun `classes with 'UseCase' suffix should have single 'public operator' method named 'invoke'`() {
    Konsist
        .scopeFromProject()
        .classes()
        .withNameEndingWith("UseCase")
        .assertTrue {
            val hasSingleInvokeOperatorMethod = it.containsFunction { function ->
                function.name == "invoke" && function.hasPublicOrDefaultModifier && function.hasOperatorModifier
            }

            hasSingleInvokeOperatorMethod && it.countFunctions { item -> item.hasPublicOrDefaultModifier } == 1
        }
}
```

## 4. I n t e r f a c e s   W i t h   ` R e p o s i t o r y `   A n n o t a t i o n   S h o u l d   R e s i d e   I n   ` d a t a `   P a c k a g e

```kotlin
@Test
fun `interfaces with 'Repository' annotation should reside in 'data' package`() {
    Konsist
        .scopeFromProject()
        .interfaces()
        .withAnnotationOf(Repository::class)
        .assertTrue { it.resideInPackage("..data..") }
}
```

## 5. E v e r y   U s e C a s e   C l a s s   H a s   T e s t

```kotlin
@Test
fun `every UseCase class has test`() {
    Konsist
        .scopeFromProduction()
        .classes()
        .withParentNamed("UseCase")
        .assertTrue { it.hasTestClass() }
}
```

