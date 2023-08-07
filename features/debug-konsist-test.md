---
description: Understand the test
---

# Debug Konsist Test

There are multiple tools that facilitate debugging and help understand what's going on inside the test.&#x20;

## Exalate Expression

The [IntelliJ IDEA](https://www.jetbrains.com/idea/) / and [Android Studio](https://developer.android.com/studio) provide a handy feature called [ Evaluate Expressions](https://www.jetbrains.com/help/rider/Evaluating\_Expressions.html#eval-expression-dialog) that is ideal for debugging Konsist tests.&#x20;

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

This is just starting point that helps to review data exposed by konsist.

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
    .classes() // List<KoClass>
    .print()
```

Print single declaration:

```kotlin
koScope
    .classes()
    .first() // KoClass
    .print()
```





