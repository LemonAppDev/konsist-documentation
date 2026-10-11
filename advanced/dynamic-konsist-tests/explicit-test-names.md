# Explicit Test Names

For dynamic tests, Konsist can't obtain the name of the current dynamic test from the call stack:

* JUnit 5 dynamic test - Konsist uses the name of the enclosing `@TestFactory` method, so every dynamic test created by this method shares the same name.
* Kotest - the test name is not available, so it is omitted from the error message (e.g. `Assert was violated (1 time)`) and suppression by test name is not possible.

Test name may be correctly displayed in the IDE, however, Konsist needs the test name to enable:

* Correct test names are displayed in the log when the test is failing
* Test suppression (See [suppressing-konsist-test.md](../../writing-tests/suppressing-konsist-test.md "mention"))

The test name can be provided in two ways:

* Register a test framework extension once - `KoJUnitExtension` (JUnit 5) or `KoKotestExtension` (Kotest 6). Konsist then reads the name of the running test, so the name does not have to be repeated in every assertion. Available since Konsist `0.19.0`.
* Pass the `testName` argument to every assertion.

{% hint style="info" %}
See [.](./ "mention").
{% endhint %}

The `testName` argument should be passed to `assertX` methods such as `assertTrue`, `assertFalse`, etc. Let's look at the code:

```kotlin
Konsist.scopeFromProject()
    .classes()
    .assertTrue(testName = "My test name") { ... } //passed test name
```

Here is the summary of test frameworks:

| Testing Framework | Determination | Test name without extension and `testName` | Extension            | Pass testName?                    |
| ----------------- | ------------- | ------------------------------------------ | -------------------- | --------------------------------- |
| JUnit4            | static        | test method name                           | -                    | Not required                      |
| JUnit5            | static        | test method name                           | -                    | Not required                      |
| JUnit5            | dynamic       | `@TestFactory` method name                 | `KoJUnitExtension`   | Not required with extension       |
| Kotest 6          | dynamic       | none (omitted)                             | `KoKotestExtension`  | Not required with extension       |
| Kotest 5          | dynamic       | none (omitted)                             | not supported        | Recommended                       |

{% hint style="info" %}
The `testName` argument always takes precedence over the name provided by the extension.
{% endhint %}

Here is a concrete implementation for each test framework:

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

{% tab title="JUnit 5 (dynamic test)" %}
[JUnit 5](https://junit.org/junit5/) introduced native support for dynamic tests, allowing tests to be generated at runtime through the `@TestFactory` annotation.

Register `KoJUnitExtension` to use the name of the dynamic test (the name passed to `dynamicTest`) without repeating it in the `testName` argument:

```kotlin
@ExtendWith(KoJUnitExtension::class)
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
                    useCase.assertTrue { it.hasTestClasses() }
                },
                dynamicTest("${useCase.name} should reside in ..domain.usecase.. package") {
                    useCase.assertTrue { it.resideInPackage("..domain.usecase..") }
                },
            )
        }
}
```

Instead of annotating every test class, the extension can be registered for all tests by enabling JUnit [extension auto-detection](https://junit.org/junit5/docs/current/user-guide/#extensions-registration-automatic) (Konsist registers `KoJUnitExtension` for auto-detection). Add this line to the `src/test/resources/junit-platform.properties` file:

```properties
junit.jupiter.extensions.autodetection.enabled=true
```

With the extension registered, a dynamic test can be suppressed by the dynamic test name (`@Suppress("konsist.MyUseCase should have test")`) or by the `@TestFactory` method name (`@Suppress("konsist.use case test")`), which suppresses all dynamic tests created by this method.

Without the extension pass the `testName` argument to every assertion:

```kotlin
dynamicTest("${useCase.name} should have test") {
    useCase.assertTrue(testName = "${useCase.name} should have test") {
        it.hasTestClasses()
    }
}
```
{% endtab %}

{% tab title="Kotest 6" %}
[Kotest](https://kotest.io/) provides robust support for dynamic tests, allowing developers to define test cases programmatically at runtime, making it a flexible alternative to traditional JUnit testing.

Register `KoKotestExtension` (see [kotest-support.md](../../features/kotest-support.md "mention") for all registration options) to use the Kotest test name without the `testName` argument:

```kotlin
@ApplyExtension(KoKotestExtension::class)
class SampleDynamicKonsistTest : FreeSpec({
    Konsist
        .scopeFromProject()
        .classes()
        .withNameEndingWith("UseCase")
        .forEach { useCase ->
            "${useCase.name} should have test" {
                useCase.assertTrue { it.hasTestClasses() }
            }
            "${useCase.name} should reside in ..domain.usecase.. package" {
                useCase.assertTrue { it.resideInPackage("..domain.usecase..") }
            }
        }
})
```

Without the extension utilize the name derived from the Kotest context (`this.testCase.name.name`) as the value for the `testName` argument:

```kotlin
"${useCase.name} should have test" {
    useCase.assertTrue(testName = this.testCase.name.name) { it.hasTestClasses() }
}
```
{% endtab %}

{% tab title="Kotest 5" %}
`KoKotestExtension` is not supported in Kotest 5. Utilize the name derived from the Kotest context (`this.testCase.name.testName`) as the value for the `testName` argument:

```kotlin
class SampleDynamicKonsistTest : FreeSpec({
    Konsist
        .scopeFromProject()
        .classes()
        .withNameEndingWith("UseCase")
        .forEach { useCase ->
            "${useCase.name} should have test" {
                useCase.assertTrue(testName = this.testCase.name.testName) { it.hasTestClasses() }
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

