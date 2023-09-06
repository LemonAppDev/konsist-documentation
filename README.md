# What is Konsist?

![](.gitbook/assets/konsist-logo.png)

Konsist is a static code analyzer for the [Kotlin](https://kotlinlang.org/) language compatible with all types of Kotlin projects including [Kotlin Multiplatform](https://kotlinlang.org/docs/multiplatform.html). Konsist facilitates codebase standardization by enforcing coding conventions and guarding the project architecture. Konsist enables writing consistency checks in the form of unit tests. These tests are intended to be executed at the pull request (PR) level as a verification check.

{% hint style="info" %}
Konsist is in the early stage of development. See the [project-status.md](getting-started/project-status.md "mention").
{% endhint %}

Konsist provides two types of checks to comprehensively assess the codebase - declaration-level check and architecture-level check.

## Declaration Checks

The first type involves declaration checks, where custom tests are created to identify common issues and violations at the declaration level (classes, functions, properties, etc.). These cover various aspects such as class naming, package structure, visibility modifiers, presence of annotations, etc. Here are a few ideas of things to check:

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
        .withNameEndingWith("UseCase") // Filter classes heaving name ending with 'UseCase'
        .assert { it.resideInPackage("..domain.usecase..") } // Assert that each class resides in 'any domain.usecase any' package
}
```

{% hint style="info" %}
For more KOnsist test samples see the [snippets](inspiration/snippets/ "mention")section.
{% endhint %}

## Architecture Checks

The second type of Konsit check revolves around communication between layers, intended to maintain the separation of concerns and improve maintainability and flexibility. eg:

* The `domain` layer is independent
* The `data` layer depends on `domain` layer
* The `presentation` layer depends on `domain` layer
* etc.

{% hint style="info" %}
These types of checks are useful when the architecture layer is defined by the package, rather than a module where dependencies can be enforced by the build system.
{% endhint %}

Here is a sample test that verifies if [Clean Architecture](https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html) dependency requirements are valid:

```kotlin
@Test
fun `clean architecture layers have correct dependencies`() {
    Konsist
        .scopeFromProject() // Define the scope containing all Kotlin files present in project
        .assertArchitecture { // Assert architecture
            // Define layers
            val domain = Layer("Domain", "com.myapp.domain..")
            val presentation = Layer("Presentation", "com.myapp.presentation..")
            val data = Layer("Data", "com.myapp.data..")

            // Define architecture assertions
            domain.dependsOnNothing()
            presentation.dependsOn(domain)
            data.dependsOn(domain)
        }
} 
```
