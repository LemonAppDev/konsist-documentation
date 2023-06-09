---
description: Quickly configure Konsist and run the first test.
---

# Quick Start

The following example provides a glimpse at the minimum requirements for writing a test in Konsist. The subsequent pages of this section will provide further details on all available features and [compatibility.md](compatibility.md "mention").

{% hint style="info" %}
Starter (preconfigured) projects containing Konsist tests are available in the [project repository](https://github.com/LemonAppDev/konsist/tree/main/samples/starter-projects).
{% endhint %}

### Add Repository

Add `mavenCentral` repository:

```
repositories {
    mavenCentral()
}
```

### Add Konsist Dependency

To use Konsist, include the Konsist dependency from Maven Central:

{% tabs %}
{% tab title="Gradle (Kotlin)" %}
Add the following dependency to the `module\build.gradle.kts` file:

```kotlin
dependencies {
    testImplementation("com.lemonappdev:konsist:0.8.0")
}
```
{% endtab %}

{% tab title="Gradle (Groovy)" %}
Add the following dependency to the `module\build.gradle` file:

```groovy
dependencies {
    testImplementation "com.lemonappdev:konsist:0.7.9"
}
```
{% endtab %}

{% tab title="Maven" %}
Add the following dependency to the `module\pom.xml` file:

```xml
<dependency>
    <groupId>com.lemonappdev</groupId>
    <artifactId>konsist</artifactId>
    <version>0.7.9</version>
    <scope>test</scope>
</dependency>
```
{% endtab %}

{% tab title="More" %}
Dependency can be added to other build systems as well. Check the [snippets](https://central.sonatype.com/artifact/com.lemonappdev/konsist) section in the sonatype repository.&#x20;
{% endtab %}
{% endtabs %}

{% hint style="info" %}
To achieve better test separation Konsist can be configured inside `konsistTest` source set or dedicated module. See [isolate-konsist-tests.md](../advanced/isolate-konsist-tests.md "mention").
{% endhint %}

## High-Level Picture

At a high-level Konsist check is a Unit test following multiple implicit steps.

3 steps are required for a _declaration check_ and 2 steps are required for an _architecture check_:

```mermaid
%%{init: {'theme':'forest'}}%%
flowchart TB
    Step1["1. Create The Scope"]-->StepD2
    Step1["1. Create The Scope"]-->StepA2
    StepD2["2. Query and Filter The Declarations"]-->StepD3
    StepD3["3. Assert"]
    StepA2["2. Assert Architecture"]
    style Step1 fill:#52B523,stroke:#666,stroke-width:2px,color:#fff
```

### Create The Scope

The first step is to get a list of Kotlin files to be verified. This step is common for declaration checks and architecture checks.&#x20;

The `Konsist` object is an entry point to the Konsist framework. The `scopeFromProject` method allows to obtain the instance of the scope containing all Kotlin project files:

```kotlin
Konsist.scopeFromProject()
```

{% hint style="info" %}
To define more granular scopes see the [koscope.md](../writing-tests/koscope.md "mention") page.
{% endhint %}

The following sections will present how to follow up by writing declaration checks and architecture checks.

## Declaration Check

Let's write a simple test to verify that classes (class declarations) annotated with the `RestController` annotation resides in `controller` package.

### Query and Filter Declarations

To write this declaration check query all present in the scope:

```kotlin
Konsist.scopeFromProject()
    .classes()

```

Perform additional filtering to get classes annotated with `RestController` annotation:

```kotlin
Konsist.scopeFromProject()
    .classes()
    .withAnnotation<RestController>
```

{% hint style="info" %}
To perform more advanced querying and filtering see the [declaration-query-and-filter.md](../writing-tests/declaration-query-and-filter.md "mention")page.
{% endhint %}

### Assert

Assert is the final step to perform declaration verification - use `assert` combined with  `koClass.resideInPackage` method to make sure that all classes (filtered in the previous step) reside in `controller` package:

```kotlin
Konsist.scopeFromProject()
    .classes()
    .withAnnotationsOf<RestController>
    .assert { it.resideInPackage("..controller") }
```

{% hint style="info" %}
To learn more about assertions see [declaration-assert.md](../writing-tests/declaration-assert.md "mention") page.
{% endhint %}

{% hint style="info" %}
The double dot syntax (`..)` means zero or more packages - controller package preceded by any number of kages (see[packageselector.md](../features/packageselector.md "mention") syntax).
{% endhint %}

### Wrap Konsist Code In JUnit Test

The above code describes declaration consistency logic. To guard this logic (and ideally, check it with every [Pull Request](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/about-pull-requests)) it will be executed in form of a unit test:

```kotlin
class ControllerClassKonsistTest {
    @Test
    fun `classes annotated with 'RestController' annotation reside in 'controller' package`() {
        Konsist.scopeFromProject() // 1. Create a scope representing the whole project (all Kotlin files in project)
            .classes() // 2. Get all classes in the project
            .withAnnotationsOf<RestController> // 2. Filter classes annotated with 'RestController'
            .assert { it.resideInPackage("..controller..") } // 3. Define the assertion
    }
}
```

The above snippet presents a complete example of a test verifying that all classes annotated with `RestController` annotation reside in the `controler` package. The test will verify existing nad new classes.

{% hint style="info" %}
This test is written using [JUnit](https://junit.org/) testing framework.
{% endhint %}

## Architecture Check

Let's write a simple test to verify that application architecture rules are preserved. In this scenario, the application follows simple 3-layer architecture, where `Presentation` layer depends on `Business`  layer and `Business` layer depends on `Data` layer. The `Data` layer has no layer dependencies.

```mermaid
%%{init: {'theme':'forest'}}%%
flowchart LR
    Presentation["Presentation Layer"]-->Business
    Business["Business Layer"]-->Data
    Data["Data Layer"]
```

### Assert Architecture

Assert architecture is the final step for architecture verification - use `assertArchiteture` combined with architecture definition to make sure that all classes meet architectural criteria:

```kotlin
Konsist
    .scopeFromProject() // Define the scope containing all Kotlin files present i
    .assertArchitecture { // Assert architecture
        // Define layers
        private val presentation = Layer("Presentation", "com.myapp.presentation..")
        private val business = Layer("Business", "com.myapp.business..")
        private val data = Layer("Data", "com.myapp.data..")

        // Define architecture assertions
        presentation.dependsOn(business)
        business.dependsOn(data)
        data.dependsOnNothing()
    }
```

### Wrap Konsist Code In JUnit Test

The above code describes architecture consistency logic. Same as with declaration check this logic should be executed as a unit test:

```kotlin
@Test
fun `architecture layers have dependencies correct`() {
    Konsist
        .scopeFromProject() // Define the scope containing all Kotlin files present i
        .assertArchitecture { // Assert architecture
            // Define layers
            private val presentation = Layer("Presentation", "com.myapp.presentation..")
            private val business = Layer("Business", "com.myapp.business..")
            private val data = Layer("Data", "com.myapp.data..")

            // Define architecture assertions
            presentation.dependsOn(business)
            business.dependsOn(data)
            data.dependsOnNothing()
        }
}
```

For more tests check the samples in the [Broken link](broken-reference "mention") section.

## Additional JUnit5 Setup

By default, JUnit tests are run sequentially in a single thread. To speed up tests parallel execution can be enabled.&#x20;



Create `junit-platform.properties` a file containing:&#x20;

```properties
junit.jupiter.execution.parallel.enabled=true
junit.jupiter.execution.parallel.mode.default=concurrent
junit.jupiter.execution.parallel.config.strategy=dynamic
junit.jupiter.execution.parallel.config.dynamic.factor=0.95
```

Place this file in the `resources`  directory of the test source set e.g:

```
src/test/resource/junit-platform.properties

or

src/konsistTest/resource/junit-platform.properties
```

Read more in the official [JUnit5 documentation](https://junit.org/junit5/docs/5.3.0-M1/user-guide/index.html#writing-tests-parallel-execution).



