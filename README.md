# What is Konsist?

![](.gitbook/assets/konsist-logo.png)

Konsist is a static code analyzer for [Kotlin](https://kotlinlang.org/) language. Konsist facilitates codebase standardization by enforcing coding conventions and guarding the project architecture. Konsist enables writing consistency checks in the form of unit tests. These tests are intended to run as a PR-level check.

Konsist provides two types of checks to comprehensively assess the codebase - declaration check and architecture check.

{% hint style="info" %}
Konsist is still in the early stage of development. See the [project-status.md](getting-started/project-status.md "mention").
{% endhint %}

## Declaration Checks

The first type involves declaration checks, where custom tests are created to identify common issues and violations at the declaration level (class, functions, properties, etc.). These cover various aspects such as class naming, package structure, annotations, naming, etc. Here are a few ideas for the checks:

* Every child class extending `ViewModel` must have `ViewModel` suffix
* Classes with the `@Repository` annotation should reside in `..repository..` package
* Every class constructor has alphabetically ordered parameters
* Every constructor parameter has a name derived from the class name
* Field injection and `m` prefix is forbidden
* Every public member in `api` package must be documented with kDoc
* and more...

Here is a sample test that verifies if every use case class resides in `domain.usecase` package:

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

## Architecture Checks

In a layered software architecture, communication between layers usually follows specific rules and principles, intended to maintain separation of concerns and improve maintainability and flexibility.&#x20;

The second type of check revolves around evaluating the application's layering, modularity, coupling, and compliance with architectural patterns eg:

* The `domain` layer is independent
* The `data` layer depends on `domain` layer
* etc.

Here is a sample test that verifies if [Clean Architecture](https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html) dependencies are correct:

```kotlin
@Test
fun `clean architecture dependencies are correct`() {
    Konsist
        .scopeFromProject() // Define the scope containing all Kotlin files present i
        .assertArchitecture { // Assert architecture
            // Define layers
            private val domain = Layer("Domain", "com.myapp.domain..")
            private val presentation = Layer("Presentation", "com.myapp.presentation..")
            private val data = Layer("Data", "com.myapp.data..")

            // Define architecture assertions
            domain.dependsOnNothing()
            presentation.dependsOn(domain)
            data.dependsOn(domain)
        }
}
```

## Whats Next?

Look at the [gettingstarted.md](getting-started/gettingstarted.md "mention") page to learn how to set up Konsist or go straight to the [Broken link](broken-reference "mention") section to review more examples of Konsist tests.&#x20;
