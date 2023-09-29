# KoTest Snippets

Whats new new new

## 1. Use Case Test

```kotlin
class UseCaseTest : FreeSpec({
    "UseCase has test class" {
        Konsist
            .scopeFromProject()
            .classes()
            .withNameEndingWith("UseCase")
            .assertTrue { it.hasTestClass() }
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
                useCase.assertTrue { it.hasTestClass() }
            }
            "${useCase.name} should reside in ..domain..usecase.. package" {
                useCase.assertTrue { it.resideInPackage("..domain..usecase..") }
            }
            "${useCase.name} should ..." {
                // another Konsist assert
            }
        }
})
```

