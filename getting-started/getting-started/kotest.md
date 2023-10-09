# KoTest Support

Konsist has no way of retrieving the name of the current [KoTest](https://kotest.io/) test (unlike JUnit).  To allow suppression (and correct test names) it is recommended to utilize the name derived from the KoTest context using `testName` argument:

```kotlin
class UseCaseTest : FreeSpec({
    "useCase test" {
        Konsist
            .scopeFromProject()
            .classes()
            .assertTrue (testName = this.testCase.name.testName) {  }
    }
})
```

To facilitate test name retrieval you can add this custom extension:

```kotlin
val TestScope.koTestName: String
    get() = this.testCase.name.testName
    
// .assertTrue (testName = koTestName) {  }
```
