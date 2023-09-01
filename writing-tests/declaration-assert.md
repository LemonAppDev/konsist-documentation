---
description: Verify codebase using Konsist API
---

# Declaration Assert

Assertions are used to perform code base verification. This is the final step of Konsist verification preceded by scope creation ([koscope.md](koscope.md "mention")) and [declaration-query-and-filter.md](declaration-query-and-filter.md "mention") steps:

```mermaid
%%{init: {'theme':'forest'}}%%
flowchart TB
    Step1["1. Create The Scope"]-->Step2
    Step2["2. Query and Filter The Declarations"]-->Step3
    Step3["3. Assert"]
    style Step3 fill:#52B523,stroke:#666,stroke-width:2px,color:#fff
```

## Assert

In the below snippet, the assertion (performed on the list of interfaces) verifies if every interface has a `public` visibility modifier.

```kotlin
koScope
    .interfaces()
    .assert { it.hasPublicModifier() }
```

The `it` parameter inside the `assert` the method represents a single declaration (single interface in this case), however, the assertion itself will be performed on every available interface. The last line in the `assert` block will be evaluated as `true` or `false` providing the result for a given assert.

## Assert Not

The `assertNot` is a negation of the `assert` method. In the below snippet, the assertion (performed on the list of properties) verifies if none of the properties has the `Inject` annotation:

```kotlin
koScope
    .classes()
    .properties()
    .assertNot { it.hasAnnotationOf<Inject>() }
```
