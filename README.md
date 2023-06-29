# What is Konsist?

![](.gitbook/assets/konsist-logo.png)

Konsist is a static code analyzer for [Kotlin](https://kotlinlang.org/) language. Konsist facilitates the standardization of the Kotlin codebase by enforcing coding conventions and guarding project consistency. Here are few ideas for the checks e.g.:

* Every child class extending `ViewModel` must have `ViewModel` suffix
* Classes with the `@Repository` annotation should reside in `..repository..` package
* Every class constructor has alphabetically ordered parameters
* Every constructor parameter has a name derived from the class name
* Field injection is forbidden
* No field should have `m` prefix
* Every public member in `api` package must be documented with kDoc
* and more...

Konsis allows writing architecture-level checks in the form of unit tests. Here is a sample test that verifies if every use case class resides in `domain.usecase` package:

```kotlin
@Test
fun `every use case reside in use case package`() {
    Konsist
        .scopeFromProject() // Define the scope containing all Kotlin files present in the project
        .classes() // Get all class declarations
        .withNameSuffix("UseCase") // Filter classes heaving name ending with 'UseCase'
        .assert { it.resideInPackages("..domain.usecase..") } // Assert that each class resides in 'any domain.usecase any' package
}
```

Konsist is intended to run as a PR-level check, similar to other tests and linters.&#x20;

{% hint style="info" %}
Konsist is still in the early stage of development. See the [project-status.md](getting-started/project-status.md "mention").
{% endhint %}

Look at the [gettingstarted.md](getting-started/gettingstarted.md "mention") page to learn how to set up Konsist or go straight to the [Broken link](broken-reference "mention") section to review examples of Konsist tests.&#x20;
