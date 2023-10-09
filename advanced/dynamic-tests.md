---
description: From static to dynamic
---

# Dynamic Tests

In the realm of Konsist tests, the default behavior runs multiple validations within a single test. When working with an application that has 3 use cases, one might question the best approach to verify if each use case adheres to specific criteria. Consider this application heaving 3 use cases:

<figure><img src="../.gitbook/assets/image (16).png" alt=""><figcaption></figcaption></figure>

We want to verify if every use case follows certain rules:

* if every use case has a test
* if every use case is in `domain.usecase` package

A typical approach would be to write two Konsist tests:

{% tabs %}
{% tab title="JUnit" %}
```kotlin
class UseCaseKonsistTest {
    @Test
    fun `use case should have test`() {
        Konsist
            .scopeFromProject()
            .classes()
            .withNameEndingWith("UseCase")
            .assertTrue { it.hasTestClass() }
    }

    @Test
    fun `use case reside in domain dor usecase package`() {
        Konsist
            .scopeFromProject()
            .classes()
            .withNameEndingWith("UseCase")
            .assertTrue { it.resideInPackage("..domain..usecase..") }
    }
}
```
{% endtab %}

{% tab title="Kotest" %}
```kotlin
class UseCaseKonsistTest : FreeSpec({
    val useCases = Konsist
        .scopeFromProject()
        .classes()
        .withNameEndingWith("UseCase")

    "use case should have test" {
        useCases.assertTrue(testName = this.testCase.name.testName) { it.hasTestClass() }
    }

    "use case should reside in ..domain.usecase.. packag" {
        useCases.assertTrue(testName = this.testCase.name.testName) { it.resideInPackage("..domain.usecase..") }
    }
})
```
{% endtab %}
{% endtabs %}

These tests would produce output in the IDE:

<figure><img src="../.gitbook/assets/image (18).png" alt=""><figcaption></figcaption></figure>

This setup can be optimized further. Rather than running just two tests, you can structure it so that each test assertion becomes its own standalone test, resulting in six distinct tests (3 use cases times two tests):

* `CategorizeGroceryItemsUseCase should have test`
* `CategorizeGroceryItemsUseCase should reside in ..domain..usecase.. package`
* `AdjustCaloricGoalUseCase should have test`
* `AdjustCaloricGoalUseCase should reside in ..domain..usecase.. package`
* `CalculateDailyIntakeUseCase should have test`
* `CalculateDailyIntakeUseCase should reside in ..domain..usecase.. package`

Using this technique, it becomes easier to spot tests that have failed. Let's convert the above test into dynamic tests:

{% tabs %}
{% tab title="JUnit 5" %}


```kotlin
class UseCaseKonsistTest {
    @TestFactory
    fun `use case test`(): Stream<DynamicTest> = Konsist
        .scopeFromProject()
        .classes()
        .withNameEndingWith("UseCase")
        .stream()
        .flatMap { useCase ->
            Stream.of(
                dynamicTest("${useCase.name} should have test") {
                    useCase.assertTrue {
                        it.hasTestClass()
                    }
                },
                dynamicTest("${useCase.name} should reside in ..domain.usecase.. package") {
                    useCase.assertTrue {
                        it.resideInPackage("..domain.usecase..")
                    }
                },
            )
        }
}
```

The IDE will display the tests as follows:

<figure><img src="../.gitbook/assets/image (19).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
In JUnit 4, the concept of dynamic tests (like JUnit 5's `@TestFactory`) does not exist natively.
{% endhint %}
{% endtab %}

{% tab title="Kotest" %}
Kotest offers native support for JUnit's dynamic tests right from the get-go. This means that developers can effortlessly integrate and utilize dynamic testing features without needing additional configurations or plugins.

```kotlin
class UseCaseKonsistTest : FreeSpec({
    Konsist
        .scopeFromProject()
        .classes()
        .withNameEndingWith("UseCase")
        .forEach { useCase ->
            "${useCase.name} should have test" {
                useCase.assertTrue(testName = this.testCase.name.testName) { it.hasTestClass() }
            }
            "${useCase.name} should reside in ..domain.usecase.. package" {
                useCase.assertTrue(testName = this.testCase.name.testName) { it.resideInPackage("..domain..usecase..") }
            }
        }
})
```

The IDE will display the tests as follows:

<figure><img src="../.gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>
{% endtab %}
{% endtabs %}

By using dynamic tests instead of just two static ones, developers can offer more precise verifications for each use case. This not only makes failures easier to spot but also improves test organization and readability.



