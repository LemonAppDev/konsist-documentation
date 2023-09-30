# Getting Started

The following example provides the minimum setup for writing a Konsist test.&#x20;

{% hint style="info" %}
Starter (preconfigured) projects containing Konsist tests are available [here](https://github.com/LemonAppDev/konsist/tree/main/samples/starter-projects).
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
    testImplementation("com.lemonappdev:konsist:0.12.2")
}
```
{% endtab %}

{% tab title="Gradle (Groovy)" %}
Add the following dependency to the `module\build.gradle` file:

```groovy
dependencies {
    testImplementation "com.lemonappdev:konsist:0.12.2"
}
```
{% endtab %}

{% tab title="Maven" %}
Add the following dependency to the `module\pom.xml` file:

```xml
<dependency>
    <groupId>com.lemonappdev</groupId>
    <artifactId>konsist</artifactId>
    <version>0.12.2</version>
    <scope>test</scope>
</dependency>
```
{% endtab %}

{% tab title="More" %}
Dependency can be added to other build systems as well. Check the [snippets](https://central.sonatype.com/artifact/com.lemonappdev/konsist) section in the Sonatype repository.&#x20;
{% endtab %}
{% endtabs %}

{% hint style="info" %}
To achieve better test separation Konsist can be configured inside a `konsistTest` source set or a dedicated module. See [isolate-konsist-tests.md](../../advanced/isolate-konsist-tests.md "mention").
{% endhint %}

## High-Level Picture

At a high-level Konsist check is a Unit test following multiple implicit steps.

3 steps are required for a _declaration check_ and 4 steps are required for an _architecture check_:

```mermaid
%%{init: {'theme':'forest'}}%%
flowchart TB
    Step1["1. Create The Scope"]-->StepD2
    Step1["1. Create The Scope"]-->StepA2
    StepD2["2. Query and Filter The Declarations"]-->StepD3
    StepD3["3. Assert"]
    StepA2["2. Assert Architecture"]-->StepA3
    StepA3["2a. Define Layers"]-->StepA4
    StepA4["2b. Define Architecture Assertions"]
    style Step1 fill:#52B523,stroke:#666,stroke-width:2px,color:#fff
```

{% hint style="info" %}
The declaration represents Kotlin declaration eg. Kotlin class is represented by `KoClassDeclaration` allowing to access class name (`koClassDeclaration.name`), methods (`koClassDeclaration.functions()`), etc. See [declaration.md](../../features/declaration.md "mention").&#x20;
{% endhint %}

### Create The Scope

The first step is to get a list of Kotlin files to be verified. This step is common for declaration checks and architecture checks.&#x20;

The `Konsist` object is an entry point to the Konsist framework. The `scopeFromProject` method obtains the instance of the scope containing all Kotlin project files present in the project:

```kotlin
Konsist.scopeFromProject() // Define the scope containing all Kotlin files present in the project
```

{% hint style="info" %}
To define more granular scopes see the [koscope.md](../../writing-tests/koscope.md "mention") page.
{% endhint %}

The following sections will present how to use a scope by writing declaration checks and architecture checks.

## Declaration Check

Let's write a simple test to verify that classes (class declarations) annotated with the `RestController` annotation resides in `controller` package.

### Query and Filter Declarations

To write this declaration check query all classes present in the scope:

```kotlin
Konsist.scopeFromProject()
    .classes() // Get scope classes

```

Perform additional filtering to get classes annotated with `RestController` annotation:

```kotlin
Konsist.scopeFromProject()
    .classes()
    .withAllAnnotationsOf(RestController::class) // Filter classes annotated with 'RestController'
```

{% hint style="info" %}
To perform more advanced querying and filtering see the [declaration-query-and-filter.md](../../writing-tests/declaration-query-and-filter.md "mention")page.
{% endhint %}

### Assert

Assert is the final step to perform declaration verification - use `assert` combined with  `koClass.resideInPackage` method to make sure that all classes (filtered in the previous step) reside in `controller` package:

```kotlin
Konsist.scopeFromProject()
    .classes()
    .withAllAnnotationsOf(RestController::class)
    .assert { it.resideInPackage("..controller") } // Define the assertion
```

{% hint style="info" %}
To learn more about assertions see [declaration-assert.md](../../writing-tests/declaration-assert.md "mention") page.
{% endhint %}

{% hint style="info" %}
The double dot syntax (`..)` means zero or more packages - controller package preceded by any number of packages (see[packageselector.md](../../features/packageselector.md "mention") syntax).
{% endhint %}

### Wrap Konsist Code In JUnit Test

The above code describes declaration consistency logic. To guard this logic (and ideally, check it with every [Pull Request](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/about-pull-requests)) it will be executed in form of a unit test:

```kotlin
class ControllerClassKonsistTest {
    @Test
    fun `classes annotated with 'RestController' annotation reside in 'controller' package`() {
        Konsist.scopeFromProject() // 1. Create a scope representing the whole project (all Kotlin files in project)
            .classes() // 2. Get scope classes
            .withAllAnnotationsOf(RestController::class) // 2. Filter classes annotated with 'RestController'
            .assert { it.resideInPackage("..controller..") } // 3. Define the assertion
    }
}
```

The above snippet presents a complete example of a test verifying that all classes annotated with `RestController` annotation reside in the `controller` package. Since scope is created from all project files this test will verify existing and new classes.

{% hint style="info" %}
This test is written using [JUnit](https://junit.org/) testing framework that should also be added as a project dependency (see [starter projects](https://github.com/LemonAppDev/konsist/tree/main/samples/starter-projects)).
{% endhint %}

## Architecture Check

Let's write a simple test to verify that application architecture rules are preserved. In this scenario, the application follows simple 3-layer architecture, where `Presentation` layer depends on `Business`  layer and `Business` layer depends on `Data` layer. The `Data` layer has no layer dependencies:

```mermaid
%%{init: {'theme':'forest'}}%%
flowchart LR
    Presentation["Presentation Layer"]-->Business
    Business["Business Layer"]-->Data
    Data["Data Layer"]
```

### Assert Architecture

Use `assertArchiteture` method combined with architecture definition to make sure that all classes meet architectural criteria:

```kotlin
Konsist
    .scopeFromProject()
    .assertArchitecture { // Assert architecture

    }
```

### Define Layers

Create layers instances to represent project layers. Each `Layer` instance accepts the `name` (used for presenting architecture violation errors) and `package` used to define layers.

```kotlin
Konsist
    .scopeFromProject()
    .assertArchitecture {
        // Define layers
        private val presentation = Layer("Presentation", "com.myapp.presentation..")
        private val business = Layer("Business", "com.myapp.business..")
        private val data = Layer("Data", "com.myapp.data..")
    }
```

The `com.myapp.business..` syntax means every class inside `com.myapp.business` and all sub-packages are treated as part of the given layer.

### Define Architecture Assertions

The final step is to define the relations between each layer:

```kotlin
Konsist
    .scopeFromProject()
    .assertArchitecture {
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

The above code describes architecture consistency logic. Same as with the declaration check this logic should be executed as a unit test:

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

{% hint style="info" %}
For more Konsist tests check the samples see the [Broken link](broken-reference "mention") section.&#x20;
{% endhint %}

{% hint style="info" %}
To debug konsist tests see [debug-konsist-test.md](../../features/debug-konsist-test.md "mention") page.
{% endhint %}
