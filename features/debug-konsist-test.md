---
description: Understand whats going on
---

# Debug Konsist Test

There are multiple tools that facilitate debugging and help understand what's going on inside the test.&#x20;

## Evaluate Expression

The [IntelliJ IDEA](https://www.jetbrains.com/idea/) / [Android Studio](https://developer.android.com/studio) provides a handy feature called [ Evaluate Expressions](https://www.jetbrains.com/help/rider/Evaluating\_Expressions.html#eval-expression-dialog) that is ideal for debugging Konsist tests.&#x20;

For example, you can list all of the classes present in the scope to get the class names...

```kotlin
koScope
    .classes()
    .map { it.name }
```

..or instance of a single class declaration to view its properties:

```kotlin
koScope
    .classes()
    .withNameEndingWith("UseCase")
    .first()
    .hasTest()
```

This is just a starting point that helps to review data exposed by the Konsist.

## Print To Console

If console logs are preferred then Konsist provides an API to display desired data as console logs.

Print a list of files from `KoScope` :

```kotlin
koScope // KoScope
    .print()
```

Print declarations:

```kotlin
koScope
    .classes() // List<KoClassDeclaration>
    .print()
```

Print single declaration:

```kotlin
koScope
    .classes() // List<KoClassDeclaration>
    .first() // KoClassDeclaration
    .print()
```

Print multiple declarations:

```kotlin
koScope
    .classes() // List<KoClassDeclaration>
    .print()
    .withSomeAnnotations("Logger")
    .print() // List<KoClassDeclaration>
```

Print nested declarations:

```kotlin
koScope
    .classes() // List<KoClassDeclaration>
    .flatMap { it.constructors } // List<KoConstructorDeclaration>
    .flatMap { it.parameters } //  List<KoParameterDeclaration>
    .print()
```



