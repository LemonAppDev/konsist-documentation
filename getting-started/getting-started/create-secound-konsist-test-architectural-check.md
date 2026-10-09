# Create Second Konsist Test - Architectural Check

Konsist's `Architectural Checks` serve as a robust tool for maintaining layer isolation, enabling development teams to enforce strict boundaries between different architectural layers. Here are a few things that can be verified with Konsist:

* `domain` layer is independent
* `data` layer depends on `domain` layer
* ...

{% hint style="info" %}
See [architecture-snippets.md](../../inspiration/snippets/architecture-snippets.md "mention") section for more examples.
{% endhint %}

## Write First Architectural Check

Let's write a simple test to verify that application architecture rules are preserved. In this scenario, the application follows a simple 3-layer architecture, where `Presentation` and `Data` layers depend on `Domain` layer and `Domain` layer is independent (from these layers):

```mermaid
%%{init: {'theme':'forest'}}%%
flowchart TD
    Presentation["Presentation Layer"]-->Domain["Domain Layer"]
    Data["Data Layer"]-->Domain
```

### Overview

On a high level writing Konsist `architectural check` requires 3 steps:

```mermaid
%%{init: {'theme':'forest'}}%%
flowchart TB
    Step1["1\. Define Layers"]-->Step2
    Step2["2\. Create The Scope"]-->Step3
    Step3["3\. Assert Architecture"]
```

Let's take a closer look at each of these steps.

### 1. Define Layers

Create layer instances to represent project layers. Each `Layer` instance accepts the `name` (used for presenting architecture violation errors) and `rootPackage` used to define the layer.

```kotlin
// Define layers
private val presentationLayer = Layer("Presentation", "com.myapp.presentation..")
private val domainLayer = Layer("Domain", "com.myapp.domain..")
private val dataLayer = Layer("Data", "com.myapp.data..")
```

{% hint style="info" %}
The double dot syntax (`..`) means zero or more packages - layer is represented by the package and all of its subpackages (see [packageselector.md](../../features/packageselector.md "mention") syntax).
{% endhint %}

### 2. Create The Scope

The `Konsist` object is an entry point to the `Konsist` library.&#x20;

```kotlin
Konsist
```

The `scopeFromX` methods obtain the instance of the scope containing Kotlin project files. To get all Kotlin project files present in the project use the `scopeFromProject` method:

```kotlin
// Define layers
private val presentationLayer = Layer("Presentation", "com.myapp.presentation..")
private val domainLayer = Layer("Domain", "com.myapp.domain..")
private val dataLayer = Layer("Data", "com.myapp.data..")
 
// Define the scope containing all Kotlin files present in the project
Konsist.scopeFromProject() //Returns KoScope
```

{% hint style="info" %}
To define more granular scopes such as scope from production code or scope from single module see the [koscope.md](../../writing-tests/koscope.md "mention") page.
{% endhint %}

### 3. Assert Architecture

To perform an assertion use the `assertArchitecture` method:

<pre class="language-kotlin"><code class="lang-kotlin">// Define layers
private val presentationLayer = Layer("Presentation", "com.myapp.presentation..")
private val domainLayer = Layer("Domain", "com.myapp.domain..")
private val dataLayer = Layer("Data", "com.myapp.data..")

Konsist
    .scopeFromProject()
     // Assert architecture
    .assertArchitecture {
<strong>        // Define architectural rules
</strong>    }
</code></pre>

Utilize `dependsOn` and `dependsOnNothing` methods to validate that your project's layers adhere to the defined architectural dependencies:

```kotlin
Konsist
    .scopeFromProject()
    .assertArchitecture {
        val presentationLayer = Layer("Presentation", "com.myapp.presentation..")
        val domainLayer = Layer("Domain", "com.myapp.domain..")
        val dataLayer = Layer("Data", "com.myapp.data..")

        // Define layer dependencies
        presentationLayer.dependsOn(domainLayer)
        dataLayer.dependsOn(domainLayer)
        domainLayer.dependsOnNothing()
    }
```

## Wrap Konsist Code In Test

The architecture validation logic should be protected through automated testing. By wrapping Konsist checks within standard testing frameworks such as [JUnit](https://junit.org) or [Kotest](https://kotest.io/), you can verify these rules with each [Pull Request](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/about-pull-requests):

{% tabs %}
{% tab title="JUnit" %}
```kotlin
class ArchitectureKonsistTest {
    @Test
    fun `architecture layers have correct dependencies`() {
        Konsist
            .scopeFromProject()
            .assertArchitecture {
                val presentationLayer = Layer("Presentation", "com.myapp.presentation..")
                val domainLayer = Layer("Domain", "com.myapp.domain..")
                val dataLayer = Layer("Data", "com.myapp.data..")
        
                // Define layer dependencies
                presentationLayer.dependsOn(domainLayer)
                dataLayer.dependsOn(domainLayer)
                domainLayer.dependsOnNothing()
            }
    }
}
```

{% hint style="info" %}
The [JUnit](https://junit.org/) testing framework project dependency should be added to the project. See [starter projects](https://github.com/LemonAppDev/konsist/tree/main/samples/starter-projects) to get a complete sample project.
{% endhint %}
{% endtab %}

{% tab title="Kotest" %}
```kotlin
class ArchitectureKonsistTest : FreeSpec({
    "architecture layers have correct dependencies" {
        Konsist
            .scopeFromProject()
            .assertArchitecture(testName = this.testCase.name.name) {
                val presentationLayer = Layer("Presentation", "com.myapp.presentation..")
                val domainLayer = Layer("Domain", "com.myapp.domain..")
                val dataLayer = Layer("Data", "com.myapp.data..")

                // Define layer dependencies
                presentationLayer.dependsOn(domainLayer)
                dataLayer.dependsOn(domainLayer)
                domainLayer.dependsOnNothing()
            }
    }
})
```

{% hint style="info" %}
For Kotest to function correctly the Kotest test name has to be explicitly passed. See the [kotest-support.md](../../features/kotest-support.md "mention") page.
{% endhint %}

{% hint style="info" %}
The [Kotest](https://kotest.io/) testing framework project dependency should be added to the project. See [starter projects](https://github.com/LemonAppDev/konsist/tree/main/samples/starter-projects) to get a complete sample project.
{% endhint %}
{% endtab %}
{% endtabs %}

Note that the test class has a `KonsistTest` suffix. This is the recommended approach to name classes containing Konsist tests.

## Summary

This section described the basic way of writing a Konsist architectural test. To get a better understanding of how Konsist API works see [debug-konsist-test.md](../../features/debug-konsist-test.md "mention").&#x20;
