---
description: Understand Whats going On
---

# Debug

There are multiple tools that facilitate debugging and help understand what's going on inside the test.&#x20;

## Exalate Expression

The [IntelliJ IDEA](https://www.jetbrains.com/idea/) / and [Android Studio](https://developer.android.com/studio) provide a handy feature called [ Evaluate Expressions](https://www.jetbrains.com/help/rider/Evaluating\_Expressions.html#eval-expression-dialog) that is ideal for debugging Konsist tests.&#x20;

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
    .classes() // Seqence<KoClass>
    .print()
```

Print single declaration:

```kotlin
koScope
    .classes()
    .first() // KoClass
    .print()
```





