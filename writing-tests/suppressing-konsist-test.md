# Suppress Konsist Test

The [@Suppress](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/-suppress/) annotation serves as a powerful tool to control linters and static analysis tools. When writing the Konsist test, there might be instances where the specific guard is not applicable due to certain project-specific reasons. The `@Suppress` annotation can be used to ignore those particular issues, ensuring that the codebase still adheres to the overall linting standards.

In Konsist the `@Suppress` annotation parameter name is derived from the name of the test to be suppressed. For example - this test verifies if every API declaration has KDoc:

```kotlin
fun `every api declaration has KDoc`() {
    // ...
}
```

The name of the test is `every api declaration has KDoc`, so we can suppress this test by using one of these arguments:

* The name of the actual Konsist test - `@Suppress("every api declaration has KDoc")`
* The name of the actual Konsist test prefixed with `konsist.` - `@Suppress("konsist.every api declaration has KDoc")`. This is helpful if you have multiple linters in the project and want to know which linters own a given check (check that needs to be suppressed)

In the below example, `@Suppress` annotation is applied to `author` property:

```kotlin
package com.myapp.api

/**
 * Represents a simple `Book` entity.
 */
interface Book {
    
    /**
     * The title of the book.
     */
    val title: String
    
    @Suppress("konsist.every api declaration has KDoc")
    val author: String
}
```

{% hint style="info" %}
Suppression without `konsist.` prefix also works (`@Suppress("every api declaration has KDoc")`). However, using the `konsist.` prefix is advised as it links the `Suppress` annotation to a specific tool. This ensures clarity on whether a particular suppression relates to a Konsist test.
{% endhint %}

When using `@Suppress` annotation, it's advisable to apply it to the smallest possible scope to ensure that only the intended warnings are suppressed, so other potential issues aren't inadvertently overlooked. In the above example, the `@Suppress` annotation was applied to the property.

If broader suppression is necessary, you can then escalate to the interface level:

```kotlin
package com.myapp.api

/**
 * Represents a simple `Book` entity.
 */
@Suppress("konsist.every api declaration has KDoc")
interface Book {
    
    /**
     * The title of the book.
     */
    val title: String
    
    val author: String
}
```

As a last resort, if multiple elements in a file need the same suppression, the `@Suppress` annotation can be applied to the entire file:

```kotlin
@file:Suppress("konsist.every api declaration has KDoc")
package com.myapp.api

/**
 * Represents a simple `Book` entity.
 */
interface Book {
    
    /**
     * The title of the book.
     */
    val title: String
    
    val author: String
}
```

`@Suppress` arguments can be passed in any form supported by Kotlin, e.g. `@Suppress("konsist.a", "konsist.b")`, `@Suppress(names = ["konsist.a"])` or `@kotlin.Suppress("konsist.a")`. A test name containing a comma (e.g. `@Suppress("konsist.a, b")`) is matched as a whole.

### Suppressing JUnit 5 Dynamic Test

For JUnit 5 dynamic tests the test name depends on the configuration:

| Configuration                     | Suppressed by                                                                                       |
| --------------------------------- | --------------------------------------------------------------------------------------------------- |
| `KoJUnitExtension` registered     | Dynamic test name (`@Suppress("konsist.MyUseCase should have test")`) or `@TestFactory` method name |
| `testName` argument               | `testName` value                                                                                    |
| No extension and no `testName`    | `@TestFactory` method name (`@Suppress("konsist.use case test")`)                                   |

Suppression by the `@TestFactory` method name suppresses every dynamic test created by this method. To suppress a single dynamic test register `KoJUnitExtension` or pass `testName` (see [explicit-test-names.md](../advanced/dynamic-konsist-tests/explicit-test-names.md "mention")).

### Suppressing Kotest Test

Konsist can't retrieve the name of the current [Kotest](https://kotest.io/) test from the call stack (unlike JUnit), so the test name has to be provided:

* Kotest 6 - register `KoKotestExtension` once (recommended) or pass the `testName` argument.
* Kotest 5 - pass the `testName` argument (`KoKotestExtension` is not supported).

{% hint style="info" %}
See the [kotest-support.md](../features/kotest-support.md "mention") page.
{% endhint %}

{% tabs %}
{% tab title="Kotest 6" %}
```kotlin
package com.api.test

@ApplyExtension(KoKotestExtension::class)
class UseCaseTest : FreeSpec({
    "useCase test" {
        Konsist
            .scopeFromProject()
            .classes()
            .assertTrue { ... }
    }
})
```
{% endtab %}

{% tab title="Kotest 5" %}
```kotlin
package com.api.test

class UseCaseTest : FreeSpec({
    "useCase test" {
        Konsist
            .scopeFromProject()
            .classes()
            .assertTrue(testName = this.testCase.name.testName) { ... }
    }
})
```
{% endtab %}
{% endtabs %}

To suppress such tests use the test name prefixed with `konsist.` (or the test name without the prefix):

```kotlin
package com.api.controller

@Suppress("konsist.useCase test")
class MyUseCase {
    ... // code
}
```

{% hint style="info" %}
The test name is the name of the test itself, without the names of the enclosing containers (e.g. `context`, `describe`).
{% endhint %}

{% hint style="info" %}
See [#kotestname-extension](../features/kotest-support.md#kotestname-extension "mention") to simplify the syntax for Kotest test name when using the `testName` argument.
{% endhint %}
