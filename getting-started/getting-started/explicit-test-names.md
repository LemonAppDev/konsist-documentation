# Explicit Test Names

For dynamic tests, Konsist can't obtain the current test's name. Test name may be correctly displayed in the IDE, however, the `testName` argument should be provided to enable:

* Correct test names are displayed in the log when the test is failing
* Test suppression (See [suppressing-konsist-test.md](../../writing-tests/suppressing-konsist-test.md "mention"))

{% hint style="info" %}
See [dynamic-konsist-tests.md](../../advanced/dynamic-konsist-tests.md "mention").
{% endhint %}

The `testName` argument should be passed to `assertX` methods such as `assertTrue` , `assertFalse` etc. Let's look at the code:

```kotlin
Konsist.scopeFromProject()
    .classes()
    .assertTrue(testName = "My test name") { ... } //passed test name
```

Here is the summary of test frameworks:

| Testing Framework | Determination | Pass testName? |
| ----------------- | ------------- | -------------- |
| JUnit4            | static        | Not required   |
| JUnit5            | static        | Not required   |
| JUnit5            | dynamic       | Recommended    |
| Kotest            | dynamic       | Recommended    |

Here is a concrete implementation passing he `testName` argument for each test Framework:

{% tabs %}
{% tab title="JUnit 5 (static test)" %}
[JUnit 5](https://junit.org/junit5/) introduced native support for dynamic tests, however, it also supports static tests. For static test `testName` does not have to be passed as it can be internally retrieved by Konsist.

```kotlin
@Test
fun myTest() {
    Konsist.scopeFromProject()
        .classes()
        .assertTrue { ... }
}
```
{% endtab %}

{% tab title="Junit 5 (dynamic test)" %}
[JUnit 5](https://junit.org/junit5/) introduced native support for dynamic tests, allowing tests to be generated at runtime through the `@TestFactory` annotation.

```kotlin
class SampleDynamicKonsistTest {
    @TestFactory
    fun `use case test`(): Stream<DynamicTest> = Konsist
        .scopeFromProject()
        .classes()
        .withNameEndingWith("UseCase")
        .stream()
        .flatMap { useCase ->
            Stream.of(
                dynamicTest("${useCase.name} should have test") {
                   useCase.assertTrue(testName = "${useCase.name} should have test") {
                        it.hasTestClass()
                    }
                },
                dynamicTest("${useCase.name} should reside in ..domain.usecase.. package") {
                    useCase.assertTrue(testName = "${useCase.name} should reside in ..domain.usecase.. package") {
                        it.resideInPackage("..domain.usecase..")
                    }
                },
            )
        }
}
```
{% endtab %}

{% tab title="Kotest" %}
[Kotest](https://kotest.io/) provides robust support for dynamic tests, allowing developers to define test cases programmatically at runtime, making it a flexible alternative to traditional JUnit testing. It is recommended to utilize the name derived from the Kotest (`this.testCase.name.testName`) context as the value for the `testName` argument:

```kotlin
class SampleDynamicKonsistTest : FreeSpec({
    Konsist
        .scopeFromProject()
        .classes()
        .withNameEndingWith("UseCase")
        .forEach { useCase ->
            "${useCase.name} should have test" {
                useCase.assertTrue(testName = this.testCase.name.testName) { it.hasTestClass() }
            }
            "${useCase.name} should reside in ..domain.usecase.. package" {
                useCase.assertTrue(testName = this.testCase.name.testName) { it.resideInPackage("..domain.usecase..") }
            }
        }
})
```

To facilitate test name retrieval you can add this custom `koTestName` extension:

```kotlin
val TestScope.koTestName: String
    get() = this.testCase.name.testName
```
{% endtab %}

{% tab title="JUnit 4" %}
[JUnit 4](https://junit.org/junit4/) does not natively support dynamic tests; tests in this framework are typically static and determined at compile-time, so there is no need to pass `testName` argument.

```kotlin
@Test
fun myTest() {
    Konsist.scopeFromProject()
        .classes()
        .assertTrue { ... }
}
```
{% endtab %}
{% endtabs %}

