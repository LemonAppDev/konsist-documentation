---
description: Konsist + Kotest
---

# Kotest Support

Konsist can't obtain the test name from all dynamic tests (this includes [Kotest](https://kotest.io/)).&#x20;

It's recommended to provide the test name using the `testName` parameter. By doing this:

* The appropriate test names will appear in the log if the test fails.
* Test suppression will be facilitated (See [suppressing-konsist-test.md](../../writing-tests/suppressing-konsist-test.md "mention"))

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
            .assertTrue (testName = koTestNamee) {  } // extension used
    }
})
```

{% hint style="info" %}
The above test will execute multiple assertions per test (all use cases will be verified in a single test). If you prefer better isolation and more visibility you can execute every assertion as a separate test. See the[dynamic-konsist-tests.md](../../advanced/dynamic-konsist-tests.md "mention") page.&#x20;
{% endhint %}
