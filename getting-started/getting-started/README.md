# Getting Started

The following example provides the minimum setup for defining and running a single Konsist test.

{% hint style="info" %}
Check the [starter projects](https://github.com/LemonAppDev/konsist/tree/main/samples/starter-projects) containing Konsist tests or review the [Konsist API reference](https://reference.konsist.lemonappdev.com).
{% endhint %}

###

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
The declaration represents Kotlin declaration eg. Kotlin class is represented by `KoClassDeclaration` allowing to access class name (`koClassDeclaration.name`), methods (`koClassDeclaration.functions()`), etc. See [declaration.md](../../features/declaration.md "mention").
{% endhint %}

###



##

##
