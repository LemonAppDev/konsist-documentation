# Why there are no pre-defined rules?

Many linters including [Detekt](https://github.com/detekt/detekt) and [ktlint](https://github.com/pinterest/ktlint) have a predefined set of rules. These rules are derived and aligned with guidelines or common practices for writing high-quality code and industry coding conventions ([Kotlin coding conventions](https://kotlinlang.org/docs/coding-conventions.html), [Android Kotlin style guide](https://developer.android.com/kotlin/style-guide), etc.).

However, there are no industry standards when comes to application architecture. Every code base is different - different class names, different package structures, different application layers, etc. As the project grows code base evolves as well - it tends to have more layers, more modules, and a more complex code structure. These "rules" are hard to capture by generic linter, because they are often specific to the given project.&#x20;

Let's consider a use case - a concept defined by the [Clean Architecture](https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html). At a high level the use case definition is quite simple - "use case holds a business logic". How the use case is represented in the code base? Well... In one project this may be a class that has a name ending with `UseCase`, in another, it may be a class extending `BaseUseCase` and in another class annotated with `@UseCase` annotation. The logic for filtering "all project use cases" will vary from project to project.

Now let's consider the actual structure of the use case class:&#x20;

* should every use case have `UseCase` a suffix in the class name?
* can the use case be extended or include another use case?
* should every use case reside in `usecase` the package?
* should the use case have a single method?
* how this method should be named?
* can this method have overloads or should it be defined as `invoke` an operator?
* should this method have a `suspended` modifier?
* …

Answers will vary from project to project. That is why Konsist favors a more flexible approach - it allows filtering members and defining custom code base assertions (tests). On top of that Konsist is utilizing Kotlin collection processing API to provide more control over filtering and asserting declarations ([declaration.md](../features/declaration.md "mention")).



{% hint style="success" %}
Some things can be standardized across different projects e.g. constructor parameter names being derived from the property name, or alphabetic order of the parameter. For now, custom tests will be a core part of Konsist, however, we are considering the addition of a small set of predefined rules in the future.
{% endhint %}
