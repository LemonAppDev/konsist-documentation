---
description: Konsist + Kotest
---

# Kotest Support

Konsist can't obtain the current [Kotest](https://kotest.io/) test's name, unlike JUnit. It's suggested to supply the test name using the `testName` parameter. Doing so will enable:

* Correct test names are displayed in log when the test is failing
* Test suppression (See [suppressing-konsist-test.md](../../writing-tests/suppressing-konsist-test.md "mention"))

It is recommended to utilize the name derived from the Kotest context as the value for the `testName` argument:

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

{% hint style="info" %}
This example is used [FreeSpec](https://kotest.io/docs/framework/testing-styles.html#free-spec) however Kotest provides [multiple testing styles](https://kotest.io/docs/framework/testing-styles.html).
{% endhint %}

## KoTestName Extension

To facilitate test name retrieval you can add this custom `koTestName` extension:

```kotlin
val TestScope.koTestName: String
    get() = this.testCase.name.testName
```

{% hint style="info" %}
In the future, this extension will be added to the Konsist.
{% endhint %}

This extension enables more concise syntax to provide Kotest test name:

```kotlin
class UseCaseTest : FreeSpec({
    "useCase test" {
        Konsist
            .scopeFromProject()
            .classes()
            .assertTrue (testName = koTestNamee) {  }
    }
})
```

The above test will execute multiple assertions per test (all use cases will be verified in a single test). If you prefer better isolation and more visibility you can execute every assertion as a separate test. See the[dynamic-konsist-tests.md](../../advanced/dynamic-konsist-tests.md "mention") page.&#x20;







