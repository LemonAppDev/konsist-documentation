# Suppressing Konsist Test

The [@Suppress](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/-suppress/) annotation serves as a powerful tool to control lines and static analysis tools. When writing Konsist test, there might be instances where the specific guard is not applicable due to certain project-specific reasons. The `@Suppress` annotation can be used to ignore those particular issues, ensuring that the codebase still adheres to the overall linting standards&#x20;

In Konsist the `@Suppress` annotation parameter name is derived from the name of the test of the test to be suppressed. For example - this test verifies if every API declaration has KDoc:

```
fun `every api declaration has KDoc`() {
    // ...
}
```

The name of the test is `every api declaration has KDoc`, so we can suppress this test by using one of these arguments:

* The name of the actual Konsist test  -`@Suppress("every api declaration has KDoc")`
* The name of the actual Konsist test was prefixed by `konsist` - `@Suppress("konsist.every api declaration has KDoc")`. This is helpful if you have multiple lines in the project and want to know which linters own a given check (check that needs to be suppressed)

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
Suppression without `konsist.`prefix also works (`@Suppress("every api declaration has KDoc")`). However, using the `konsist.prefix` is advised as it links the `Suppress` annotation to a specific tool. This ensures clarity on whether a particular suppression relates to a Konsist test.
{% endhint %}

When using `@Suppress` annotation, it's advisable to apply it to the smallest possible scope to ensure that only the intended warnings are suppressed, so other potential issues aren't inadvertently overlooked.  In the above example, the `@Suppress` annotation was applied to the property.&#x20;

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
    
    @Suppress("konsist.every api declaration has KDoc")
    val author: String
}
```


### Suppressing test (with KoTest)

Within Konsist, KoTest specs can use `@Suppress` annotation to make use of it.

When pairing Konsist with a [KoTest testing styles](https://kotest.io/docs/framework/testing-styles.html) for code verification, it's essential to utilize the name derived from the KoTest context.

```kotlin
...
package com.api.controller
...

@Suppress("check class with Controller ending")
class TestController {
    ... // avoided for simplicity
}

...
package com.api.test
...

class ArchitectureKonsistTest: FreeSpec({

    "check class with Controller ending" {
        Konsist
            .scopeFromProject()
            .classes()
            .withNameEndingWith("Controller")
            .assertTrue (suppressName = this.testCase.name.testName) { it.resideInPackage("..controller..") }
    }
})
```

One can note that the `.assertTrue()` function now includes a `suppressName` parameter, tapping into the suppression annotation. To leverage KoTest's context in this situation, invoke the test name as follows:

```kotlin
this.testCase.name.testName
```

{% hint style="info" %}
Here in the example, `FreeSpec` was used but any of the KoTest's Testing Style can be used.
{% endhint %}


