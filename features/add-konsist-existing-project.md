# Add Konsist To Existing Project (Baseline)

Retrofitting Konsist into a project that hasn't followed strict structural guidelines can pose initial challenges, necessitating a thoughtful approach to smoothly transition without disrupting ongoing development. Unlike most linters, which provide a [baseline file](https://developer.android.com/studio/write/lint#snapshot), Konsist follows a different methodology (for now).

{% hint style="success" %}
The baseline file will be added in the future.
{% endhint %}

There are two approaches that can be employed when retrofitting Konsist into an existing project: [#create-granular-scopes](add-konsist-existing-project.md#create-granular-scopes "mention") and [#suppress-annotation](add-konsist-existing-project.md#suppress-annotation "mention").

## Create Granular Scopes

Scope represents a set of Kotlin files. The scope allows verification of all Kotlin files in the project or only a subset of the project code base.

{% hint style="info" %}
See [koscope.md](../writing-tests/koscope.md "mention").
{% endhint %}

When refactoring an existing application, you can either choose to first refactor a module and then add a Konsist test or initially add the Konsist test to identify errors, followed by the necessary refactor. Both strategies aim to ensure modules align with Konsist's structural guidelines.

Consider the `MyDiet` application with 3 feature modules:

<figure><img src="../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

At first, the Konsist test can be applied to a single module:

```kotlin
Konsist
    .scopeFromProduction(moduleName = ":featureCaloryCalculator")
    .classes()
    .assertTrue { it.hasTestClasses() }
```

{% hint style="warning" %}
A scope created for a module that does not exist (e.g. misspelled module name) is empty and the assertion passes. Use the [strict](../writing-tests/declaration-assert.md#strict) assertion parameter to make the test fail when the declaration list is empty.
{% endhint %}

{% hint style="info" %}
To review the content of a given scope see [debug-konsist-test.md](debug-konsist-test.md "mention").
{% endhint %}

As refactoring proceeds and code gets aligned, the Konsist scope can be extended to another feature module (`featureGroceryListGenerator`):

```kotlin
val scope = Konsist.scopeFromProduction(moduleName = ":featureCaloryCalculator") +
    Konsist.scopeFromProduction(moduleName = ":featureGroceryListGenerator")

scope
    .classes()
    .assertTrue { it.hasTestClasses() }
```

When the entire code base (all modules) is aligned with the Konsist tests, the scope can be retrieved from the entire project:

```kotlin
Konsist
    .scopeFromProduction()
    .classes()
    .assertTrue { it.hasTestClasses() }
```

{% hint style="info" %}
`scopeFromProduction` is used here, because test classes themselves don't have test classes. Module scopes (`scopeFromModule`) and project scope (`scopeFromProject`) also contain test source set files.
{% endhint %}

Usage of a scope covering the entire project (e.g. `scopeFromProject` or `scopeFromProduction`) is a recommended approach because it helps to guard future modules without modifying the existing Konsist test.

Konsist provides a flexible API to create scopes from modules, source sets, packages, files, etc., and combine these scopes together. See [koscope.md](../writing-tests/koscope.md "mention").

## Suppress Annotation

The second approach, Suppress Annotation, may be helpful to add Konsist swiftly without making substantial alterations to the existing Kotlin files. See [suppressing-konsist-test.md](../writing-tests/suppressing-konsist-test.md "mention").
