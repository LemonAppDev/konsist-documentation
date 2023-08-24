#  Clean Architecture Snippets

## Clean Architecture Layers Have Correct Dependencies

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

## Classes With 'UseCase' Suffix Should Reside In 'domain' And 'usecase' Packages

```kotlin
@Test
fun `classes with 'UseCase' suffix should reside in 'domain' and 'usecase' packages`() {
        Konsist.scopeFromProject()
            .classes()
            .withNameEndingWith("UseCase")
            .assert { it.resideInPackage("..domain..usecase..") }
    }

    ```

## Classes With 'UseCase' Suffix Should Have Single Public Method Named 'invoke'

```kotlin
@Test
fun `classes with 'UseCase' suffix should have single public method named 'invoke'`() {
        Konsist.scopeFromProject()
            .classes()
            .withNameEndingWith("UseCase")
            .assert {
                val hasSingleInvokeOperatorMethod = it.containsFunction { function ->
                    function.name == "invoke" && function.hasPublicOrDefaultModifier && function.hasOperatorModifier
                }

                val hasSinglePublicDeclaration = it.numPublicDeclarations() == 1

                hasSingleInvokeOperatorMethod && hasSinglePublicDeclaration
            }
    }

    ```

## Interfaces With 'Repository' Annotation Should Reside In 'data' Package

```kotlin
@Test
fun `interfaces with 'Repository' annotation should reside in 'data' package`() {
        Konsist.scopeFromProject()
            .interfaces()
            .withAllAnnotationsOf(Repository::class)
            .assert { it.resideInPackage("..data..") }
    }

    ```

## Every UseCase Class Has Test

```kotlin
@Test
fun `every UseCase class has test`() {
        Konsist.scopeFromProduction()
            .classes()
            .withParentClass("UseCase")
            .assert { it.hasTest() }
    }
```

