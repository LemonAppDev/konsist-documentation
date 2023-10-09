# Kotest Snippets

## 1. UseCase Has Test Class

```kotlin
class KoTestSnippets {
    class UseCaseTest : FreeSpec({
        "UseCase has test class" {
            Konsist
                .scopeFromProject()
                .classes()
                .withNameEndingWith("UseCase")
                .assertTrue(testName = this.testCase.name.testName) { it.hasTestClass() }
        }
    })
}
```

## 1. 2 Use Case Tests

```kotlin
class UseCaseTests : FreeSpec({
        Konsist
            .scopeFromProject()
            .classes()
            .withNameEndingWith("UseCase")
            .forEach { useCase ->
                "${useCase.name} should have test" {
                    useCase.assertTrue(testName = this.testCase.name.testName) { it.hasTestClass() }
                }
                "${useCase.name} should reside in ..domain..usecase.. package" {
                    useCase.assertTrue(testName = this.testCase.name.testName) { it.resideInPackage("..domain..usecase..") }
                }
                "${useCase.name} should ..." {
                    // another Konsist assert
                }
            }
    })
```
