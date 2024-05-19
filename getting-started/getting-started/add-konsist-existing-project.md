# Add Konsist To Project

Retrofitting Konsist into a project that hasn't followed strict structural guidelines can pose initial challenges, necessitating a thoughtful approach to smoothly transition without disrupting ongoing development. Unlike most linters, which provide a [baseline file](https://developer.android.com/studio/write/lint#snapshot), Konsist follows a different methodology.

{% hint style="info" %}
The baseline file is considered to be added in the future.
{% endhint %}

There are two approaches that can be employed when retrofitting Konsist into an existing project[#create-more-granular-scopes](add-konsist-existing-project.md#create-more-granular-scopes "mention") and [#suppress-annotation](add-konsist-existing-project.md#suppress-annotation "mention").

## Create More Granular Scopes

Scope represents a set of Kotlin files. The scope allows to verification of all Kotlin files in the project or only a subset of the project code base.

{% hint style="info" %}
See [koscope.md](../../writing-tests/koscope.md "mention").
{% endhint %}

When refactoring an existing application, you can either choose to first refactor a module and then add a Konsist test or initially add the Konsist test to identify errors, followed by the necessary refactor. Both strategies aim to ensure modules align with Konsist's structural guidelines.

Consider this The `MyDiet` application with feature 3 modules:

<figure><img src="../../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

In the beginning, the Konsist test can be applied to a single module:

```kotlin
Konsist
    .scopeFromModule("featureCaloryCalculator")
    .classes()
    .assertTrue { it.hasTestClasses() }
```

{% hint style="info" %}
To review the content of a given scope see [debug-konsist-test.md](../../features/debug-konsist-test.md "mention").
{% endhint %}

As refactoring proceeds, this Konsist scope can be extended to another feature module (`featureGroceryListGenerator`):

```kotlin
Konsist
    .scopeFromModule("featureCaloryCalculator", "featureGroceryListGenerator")
    .classes()
    .assertTrue { it.hasTest() }
```

At some point, the entire code base (all modules) will be aligned with the Konsist test, so scope should be retrieved from the entire project:

```kotlin
Konsist
    .scopeFromProject()
    .classes()
    .assertTrue { it.hasTest() }
```

Usage of project scope (`scopeFromProject` ) is a recommended approach because it helps to guard future modules without modifying the existing Konsist test.

Konsist provides a flexible API to create scopes from modules, source sets, packages, files, etc., and combine these scopes together. See [koscope.md](../../writing-tests/koscope.md "mention").

## Suppress Annotation

The second approach, Suppress Annotation, may be helpful when to Konsist swiftly without making substantial alterations to the existing kotlin files. See [#suppress](add-konsist-existing-project.md#suppress "mention").
