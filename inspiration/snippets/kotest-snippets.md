# KoTest Snippets

## 1. Use Case Test

```kotlin
class UseCaseTest : FreeSpec({
    "UseCase has test class" {
        Konsist
            .scopeFromProject()
            .classes()
            .withNameEndingWith("UseCase")
            .assert { it.hasTestClass() }
    }
})
```

## 2. Use Case Tests

```kotlin
class UseCaseTests : FreeSpec({
    Konsist
        .scopeFromProject()
        .classes()
        .withNameEndingWith("UseCase")
        .forEach { useCase ->
            "${useCase.name} should have test" {
                useCase.assert { it.hasTestClass() }
            }
            "${useCase.name} should reside in ..domain..usecase.. package" {
                useCase.assert { it.resideInPackage("..domain..usecase..") }
            }
            "${useCase.name} should ..." {
                // another Konsist assert
            }
        }
})
```

