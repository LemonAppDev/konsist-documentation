---
description: Verify codebase using Konsist API
---

# Architecture Assertion

Architecture assertions are used to perform architecture verification. It is the final step of Konsist verification preceded by scope creation ([koscope.md](koscope.md "mention")):

```mermaid
%%{init: {'theme':'forest'}}%%
flowchart TB
    StepA2["2. Assert Architecture"]-->StepA3
    StepA3["2a. Define Layers"]-->StepA4
    StepA4["2b. Define Assertion"]
    style StepA2 fill:#52B523,stroke:#666,stroke-width:2px,color:#fff
    style StepA3 fill:#52B523,stroke:#666,stroke-width:2px,color:#fff
    style StepA4 fill:#52B523,stroke:#666,stroke-width:2px,color:#fff
```

## Assert Architecture

As an example, this simple 2-layer architecture will be used:

```mermaid
%%{init: {'theme':'forest'}}%%
flowchart LR
    Presentation["Presentation Layer"]-->Data
    Data["Data Layer"]
```

The `assertArchitecture` block defines architecture layer rules and verifies that the layer requirements are met.

```kotlin
Konsist
    .scopeFromProject()
    .assertArchitecture { 
        // Assert architecture 
    }
```

{% hint style="info" %}
The Konsist repository [contains multiple architectural tests](https://github.com/LemonAppDev/konsist/tree/main/lib/src/apiTest/kotlin/com/lemonappdev/konsist/architecture). These tests are used internally to verify Konsist API. Each test has a Konsist code and a `mermaid` diagram presenting the architecture.
{% endhint %}

## Define Layers

Create layers instances to represent project layers. Each `Layer` instance accepts the `name` (used for presenting architecture violation errors) and `package` used to define layers.

```kotlin
Konsist
    .scopeFromProject()
    .assertArchitecture {
        // Define layers
        val presentation = Layer("Presentation", "com.myapp.presentation..")
        val data = Layer("Data", "com.myapp.data..")
    }
```

{% hint style="warning" %}
The inclusion of two trailing dots indicates that the layer is denoted by the `com.myapp.business` package together with all of its sub-packages.

```kotlin
val data = Layer("Data", "com.myapp.data..") // This package only and all sub-packages
val data = Layer("Data", "com.myapp.data") // This package only
```
{% endhint %}

## Define Architecture Assertions

The final step is to define the relations between each layer:

```kotlin
koScope.assertArchitecture {
        val presentation = Layer("Presentation", "com.myapp.presentation..")
        val data = Layer("Data", "com.myapp.data..")

        presentation.dependsOn(data)
        data.dependsOnNothing()
    }
```

Architecture verification can be performed on `KoScope` (as seen above) and a list containing `KoFiles`.  For example, you can remove a few files from the scope before performing an architectural check:

```kotlin
koScope
    .files
    .filter { it.name.startsWith("Repository") }
    .assertArchitecture {
        val presentation = Layer("Presentation", "com.myapp.presentation..")
        val data = Layer("Data", "com.myapp.data..")

        presentation.dependsOn(data)
        data.dependsOnNothing()
    }
```

This approach provides more flexibility when working with complex projects, however, The desired approach is to create a dedicated scope. See [koscope.md](koscope.md "mention").

## Architecture As A Variable

Architecture configuration can be defined beforehand and stored in a variable to facilitate checks for multiple scopes:&#x20;

```kotlin
// Define architecture
val architecture = architecture {
        val presentation = Layer("Presentation", "com.myapp.presentation..")
        val data = Layer("Data", "com.myapp.data..")

        presentation.dependsOn(data)
        data.dependsOnNothing()
}

// Assert Architecture of two modules using common architecture rules
moduleFeature1Scope.assertArchitecture(architecture)
moduleFeature2Scope.assertArchitecture(architecture)
```

This approach may be helpful when refactoring existing applications. To facilitate readability the above checks should be expressed as two unit tests:

```kotlin
class ArchitectureTest {
    private val architecture = architecture {
        val presentation = Layer("Presentation", "com.myapp.presentation..")
        val data = Layer("Data", "com.myapp.data..")

        presentation.dependsOn(data)
        data.dependsOnNothing()
    }

    @Test
    fun `architecture layers of feature1 module have dependencies correct`() {
        moduleFeature1Scope.assertArchitecture(architecture)
    }
    
    @Test
    fun `architecture layers of feature2 module have dependencies correct`() {
        moduleFeature2Scope.assertArchitecture(architecture)
    }
}
```

