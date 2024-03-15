# Clean Architecture Snippets

Snippets used to guard clean architecture dependencies.

## 1. Clean Architecture Layers Have Correct Dependencies

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

## 2. Classes With `UseCase` Suffix Should Reside In `domain` And `usecase` Package

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

## 3. Classes With `UseCase` Suffix Should Have Single `public Operator` Method Named `invoke`

```kotlin
@Test
fun `classes with 'UseCase' suffix should have single 'public operator' method named 'invoke'`() {
    Konsist
        .scopeFromProject()
        .classes()
        .withNameEndingWith("UseCase")
        .assertTrue {
            val hasSingleInvokeOperatorMethod = it.hasFunction { function ->
                function.name == "invoke" && function.hasPublicOrDefaultModifier && function.hasOperatorModifier
            }

            hasSingleInvokeOperatorMethod && it.countFunctions { item -> item.hasPublicOrDefaultModifier } == 1
        }
}
```

## 4. Interfaces With `Repository` Annotation Should Reside In `data` Package

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

## 5. Every UseCase Class Has Test

```kotlin
@Test
fun `every UseCase class has test`() {
    Konsist
        .scopeFromProduction()
        .classes()
        .withNameEndingWith("UseCase")
        .assertTrue { it.hasTestClass() }
}
```

