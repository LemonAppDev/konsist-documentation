# What is Konsist?

![](.gitbook/assets/logo.png)

Konsist is a powerful static code analyzer tailored for [Kotlin](https://kotlinlang.org/), focused on ensuring codebase consistency and adherence to coding conventions. Designed to be flexible, it enables developers to set custom conventions that best fit their project's requirements. Whether you're working on  [Android](https://www.android.com/), [Spring](https://spring.io/), or [Kotlin Multiplatform](https://kotlinlang.org/docs/multiplatform.html) projects, Konsist has got you covered. By integrating seamlessly with prominent testing frameworks like JUnit4, JUnit5, and Kotest, Konsist allows developers to embed consistency checks within unit tests. These checks, which are executed during pull request reviews, act as safeguards to preserve the project's architecture.

{% hint style="info" %}
Konsist is in the early stage of development. See the [project-status.md](getting-started/project-status.md "mention").
{% endhint %}

Konsist offers two leves of checks, namely `declaration checks` and `architectural checks`, to thoroughly evaluate the codebase.

## Declaration Checks

The first type involves declaration checks, where custom tests are created to identify common issues and violations at the declaration level (classes, functions, properties, etc.). These cover various aspects such as class naming, package structure, visibility modifiers, presence of annotations, etc. Here are a few ideas of things to check:

* Every child class extending `ViewModel` must have `ViewModel` suffix
* Classes with the `@Repository` annotation should reside in `..repository..` package
* Every class constructor has alphabetically ordered parameters
* Every constructor parameter has a name derived from the class name
* Field injection and `m` prefix is forbidden
* Every public member in `api` package must be documented with KDoc
* and more...

Here is a sample test that verifies if every use case class resides in `domain.usecase` package:

{% tabs %}
{% tab title="JUnit" %}
<pre class="language-kotlin"><code class="lang-kotlin">class UseCaseKonsistTest {
    @Test
    fun `every use case reside in use case package`() {
        Konsist
            .scopeFromProject() // Define the scope containing all Kotlin files present in the project
            .classes() // Get all class declarations
            .withNameEndingWith("UseCase") // Filter classes heaving name ending with 'UseCase'
            .assertTrue { it.resideInPackage("..domain.usecase..") } // Assert that each class resides in 'any domain.usecase any' package
    }
}
<strong>
</strong></code></pre>
{% endtab %}

{% tab title="Kotest" %}
```kotlin
class UseCaseKonsistTest : FreeSpec({
    "every use case reside in use case package" {
        Konsist
        .scopeFromProject() // Define the scope containing all Kotlin files present in the project
        .classes() // Get all class declarations
        .withNameEndingWith("UseCase") // Filter classes heaving name ending with 'UseCase'
        .assertTrue (
                testName = this.testCase.name.testName
         ){ 
              it.resideInPackage("..domain.usecase..") 
         } // Assert that each class resides in 'any domain.usecase any' package
    }
})
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
For more Konsist test samples see the [snippets](inspiration/snippets/ "mention")section.
{% endhint %}

## ArchitecturalChecks

The second type of [Konsist checks](https://github.com/LemonAppDev/konsist) revolves around architecture boundaries - they are intended to maintain the separation of concerns between layers.

Consider this simple 3 layer of [Clean Architecture](https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html):

* The `domain` layer is independent
* The `data` layer depends on `domain` layer
* The `presentation` layer depends on `domain` layer
* etc.

Here is a Konsist test that verifies if Clean Architecture dependency requirements are valid:

{% tabs %}
{% tab title="JUnit" %}
<pre class="language-kotlin"><code class="lang-kotlin">class ArchitectureTest {
<strong>    @Test
</strong>    fun `clean architecture layers have correct dependencies`() {
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
}
</code></pre>
{% endtab %}

{% tab title="Kotest" %}
```kotlin
class ArchitectureTest : FreeSpec({
    "every use case reside in use case package" {
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
})
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
These types of checks are useful when the architecture layer is defined by the package, rather than a module where dependencies can be enforced by the build system.
{% endhint %}

## Summary

By utilizing Konsist, teams can be confident that their Kotlin codebase remains standardized and aligned with best practices, making code reviews more efficient and code maintenance smoother.
