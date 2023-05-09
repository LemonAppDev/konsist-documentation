# What is Konsist?

![](.gitbook/assets/konsist-logo.png)

Konsist is a library that helps to guard [Kotlin](https://kotlinlang.org/) codebase consistency. It facilitates the standardization of the Kotlin codebase by enforcing coding conventions and guarding project architecture.&#x20;

Konsist tests are written in Kotlin. Here is a simple test that verifies if every use case class resides in `domain.usecase` package:

```kotlin
@Test
fun `every use case reside in use case package`() {
    Konsist
        .scopeFromProject() // Define scope using all Kotlin files present in the project
        .classes() // Map to list of classes
        .withNameSuffix("UseCase") // Filter classes heaving name ending with 'UseCase'
        .assert { it.resideInPackages("com.app.domain.usecase") } // Assert class has com.app.domain.usecase package
}
```

{% hint style="info" %}
The `..` represents [packageselector.md](features/packageselector.md "mention") syntax.
{% endhint %}

Take a look at the [gettingstarted.md](getting-started/gettingstarted.md "mention") page to learn how to set up Konsist or go straight to the [Broken link](broken-reference "mention") section to review the examples.&#x20;
