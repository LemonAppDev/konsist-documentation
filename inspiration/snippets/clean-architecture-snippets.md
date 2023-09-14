# Clean Architecture Snippets

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
        .assert { it.resideInPackage("..domain..usecase..") }
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
        .assert {
            val hasSingleInvokeOperatorMethod = it.containsFunction { function ->
                function.name == "invoke" && function.hasPublicOrDefaultModifier && function.hasOperatorModifier
            }

            hasSingleInvokeOperatorMethod && it.numPublicOrDefaultDeclarations() == 1
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
        .assert { it.resideInPackage("..data..") }
}
```

## 5. Every UseCase Class Has Test

```kotlin
@Test
fun `every UseCase class has test`() {
    Konsist
        .scopeFromProduction()
        .classes()
        .withParentNamed("UseCase")
        .assert { it.hasTestClass() }
}
```

