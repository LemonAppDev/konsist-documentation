---
description: Konsist + Kotest
---

# Kotest Support

Konsist has first-class support for [Kotest](https://kotest.io/) meaning that every following release will be developed with Kotest compatibility in mind. API has been improved to support Kotest flows. However, Konsist cannot automatically retrieve Kotest test names, meaning the test name won't appear in error logs upon test failure. To fully utilize Konsist with Kotest, you must explicitly provide the test name.

## Setting The Test Name

Konsist can't obtain the test name from all dynamic tests (including [Kotest](https://kotest.io/) tests).

It's recommended to provide the test name using the `testName` parameter. Supplying a test name provides additional benefits:

* The appropriate test names will appear in the log if the test fails.
* Test suppression will be facilitated (See [suppressing-konsist-test.md](../writing-tests/suppressing-konsist-test.md "mention"))

{% hint style="info" %}
See [explicit-test-names.md](../advanced/dynamic-konsist-tests/explicit-test-names.md "mention") for more details.
{% endhint %}

Kotest enables fetching the test name from the context to populate the `testName` argument, ensuring consistent naming of tests:

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

To facilitate test name retrieval you can add a custom `koTestName` extension:

```kotlin
val TestScope.koTestName: String
    get() = this.testCase.name.testName
```

This extension enables more concise syntax for providing Kotest test name:

```kotlin
class UseCaseTest : FreeSpec({
    "useCase test" {
        Konsist
            .scopeFromProject()
            .classes()
            .assertTrue (testName = koTestName) {  } // extension used
    }
})
```

{% hint style="info" %}
The above test will execute multiple assertions per test (all use cases will be verified in a single test). If you prefer better isolation and more visibility you can execute every assertion as a separate test. See the[dynamic-konsist-tests](../advanced/dynamic-konsist-tests/ "mention") page.
{% endhint %}
