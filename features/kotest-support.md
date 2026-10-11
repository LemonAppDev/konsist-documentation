---
description: Konsist + Kotest
---

# Kotest Support

Konsist has first-class support for [Kotest](https://kotest.io/) meaning that every following release will be developed with Kotest compatibility in mind. API has been improved to support Kotest flows.

## Setting The Test Name

[Kotest](https://kotest.io/) tests are lambdas, so the test name is not present in the call stack and Konsist can't obtain it on its own. Without the test name:

* The error message does not contain the test name (e.g. `Assert was violated (1 time)` or `Test has failed.` for architecture assertions).
* The test can't be suppressed (See [suppressing-konsist-test.md](../writing-tests/suppressing-konsist-test.md "mention")).

How the test name is provided depends on the Kotest version:

| Kotest version | How to provide the test name                                                                        | Test name property in Kotest API |
| -------------- | --------------------------------------------------------------------------------------------------- | -------------------------------- |
| 6.x            | Register `KoKotestExtension` once (recommended) or pass the `testName` argument to every assertion | `this.testCase.name.name`        |
| 5.x            | Pass the `testName` argument to every assertion (`KoKotestExtension` is not supported)             | `this.testCase.name.testName`    |

{% hint style="info" %}
See [explicit-test-names.md](../advanced/dynamic-konsist-tests/explicit-test-names.md "mention") for more details.
{% endhint %}

## Kotest 6: KoKotestExtension

{% hint style="info" %}
`KoKotestExtension` is available since Konsist `0.19.0` and requires Kotest `6.x`.
{% endhint %}

`KoKotestExtension` passes the name of the running Kotest test to every Konsist assertion (`assertTrue`, `assertFalse`, `assertArchitecture`, etc.). The `testName` argument is no longer needed:

```kotlin
class UseCaseTest : FreeSpec({
    "useCase test" {
        Konsist
            .scopeFromProject()
            .classes()
            .assertTrue { it.resideInPackage("..domain.usecase..") } // no testName
    }
})
```

The failure message contains the test name (`Assert 'useCase test' was violated (1 time)`) and the test can be suppressed with `@Suppress("konsist.useCase test")` or `@Suppress("useCase test")`.

The extension has to be registered once. Pick one of the options:

{% tabs %}
{% tab title="Project config (all specs)" %}
Kotest 6 detects the project config class named `ProjectConfig` placed in the `io.kotest.provided` package (e.g. `src/test/kotlin/io/kotest/provided/ProjectConfig.kt`):

```kotlin
package io.kotest.provided

import com.lemonappdev.konsist.api.verify.KoKotestExtension
import io.kotest.core.config.AbstractProjectConfig

object ProjectConfig : AbstractProjectConfig() {
    override val extensions = listOf(KoKotestExtension)
}
```

To use a different location set the `kotest.framework.config.fqn` system property (e.g. `-Dkotest.framework.config.fqn=com.myapp.MyProjectConfig`).
{% endtab %}

{% tab title="Annotation (single spec)" %}
```kotlin
@ApplyExtension(KoKotestExtension::class)
class UseCaseTest : FreeSpec({
    // ...
})
```
{% endtab %}

{% tab title="Spec body (single spec)" %}
```kotlin
class UseCaseTest : FreeSpec({
    extension(KoKotestExtension)

    // ...
})
```
{% endtab %}
{% endtabs %}

Things to know:

* The test name is the name of the test itself (`this.testCase.name.name`), without the names of the enclosing containers (e.g. `context`, `describe`, `"group" - { }`). Tests with the same name inside different containers are suppressed by the same `@Suppress` value.
* The test name is available inside the test coroutine, also after switching threads (e.g. `withContext(Dispatchers.IO)`). It is not available in threads started manually inside the test.
* The `testName` argument still works and takes precedence over the name provided by the extension.

## Kotest 5: testName Argument

`KoKotestExtension` is not supported in Kotest `5.x` (Kotest `6` changed the extension API). Provide the test name using the `testName` argument. Kotest enables fetching the test name from the context to populate the `testName` argument, ensuring consistent naming of tests:

```kotlin
class UseCaseTest : FreeSpec({
    "useCase test" {
        Konsist
            .scopeFromProject()
            .classes()
            .assertTrue(testName = this.testCase.name.testName) { it.resideInPackage("..domain.usecase..") }
    }
})
```

{% hint style="info" %}
The `testName` argument can also be used with Kotest `6.x` (e.g. when the extension is not registered). Use `this.testCase.name.name` instead of `this.testCase.name.testName`.
{% endhint %}

{% hint style="info" %}
This example uses [FreeSpec](https://kotest.io/docs/framework/testing-styles.html#free-spec), however, Kotest provides [multiple testing styles](https://kotest.io/docs/framework/testing-styles.html).
{% endhint %}

### KoTestName Extension

To facilitate test name retrieval you can add a custom `koTestName` extension:

{% tabs %}
{% tab title="Kotest 5" %}
```kotlin
val TestScope.koTestName: String
    get() = this.testCase.name.testName
```
{% endtab %}

{% tab title="Kotest 6" %}
```kotlin
val TestScope.koTestName: String
    get() = this.testCase.name.name
```
{% endtab %}
{% endtabs %}

This extension enables more concise syntax for providing Kotest test name:

```kotlin
class UseCaseTest : FreeSpec({
    "useCase test" {
        Konsist
            .scopeFromProject()
            .classes()
            .assertTrue(testName = koTestName) { it.resideInPackage("..domain.usecase..") } // extension used
    }
})
```

{% hint style="info" %}
The above test will execute multiple assertions per test (all use cases will be verified in a single test). If you prefer better isolation and more visibility you can execute every assertion as a separate test. See the [dynamic-konsist-tests](../advanced/dynamic-konsist-tests/ "mention") page.
{% endhint %}
