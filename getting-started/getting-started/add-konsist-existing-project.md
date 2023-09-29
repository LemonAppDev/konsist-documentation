# Add Konsist Existing Project

Retrofitting Konsist into a project that hasn't followed strict structural guidelines can pose initial challenges, necessitating a thoughtful approach to smoothly transition without disrupting ongoing development. There are two approaches that can be employed when retrofitting Konsist [#create-more-granular-scopes](add-konsist-existing-project.md#create-more-granular-scopes "mention") and [#suppress-annotation](add-konsist-existing-project.md#suppress-annotation "mention").

## Create More Granular Scopes

Scope allows to verification of only a subset of the project code base. When refactoring application scope can be created for a single module to guard specific rules of the improved code base and then further extended to cover already refactored modules.

Consider this The `MyDiet` application with feature 3 modules:

<figure><img src="../../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

At first Konsist test can be applied to a single refactored module:

```kotlin
Konsist
    .scopeFromModule("featureCaloryCalculator")
    .classes()
    .assert { it.hasTest() }
```

Before refactoring another module (`featureGroceryListGenerator`) add it to the existing Konsist test:

```kotlin
Konsist
    .scopeFromModule("featureCaloryCalculator", "featureGroceryListGenerator")
    .classes()
    .assert { it.hasTest() }
```

Continue until all modules are aligned with the Konsist test and retrieve the scope from the entire project:

```kotlin
Konsist
    .scopeFromProject()
    .classes()
    .assert { it.hasTest() }
```

This scope will allow us to guard future modules.

{% hint style="info" %}
See [koscope.md](../../writing-tests/koscope.md "mention").
{% endhint %}

You may also join individual scopes to create a custom scope. See [#scope-creation](../../writing-tests/koscope.md#scope-creation "mention").

## Suppress Annotation

The second approach, Suppress Annotation, may be helpful when to Konsist swiftly without making substantial alterations to the existing kotlin files. See [#suppress](add-konsist-existing-project.md#suppress "mention").
