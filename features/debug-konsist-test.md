---
description: Understand whats going on
---

# Debug Konsist Test

This page will help you review the data exposed by the Konsist and understand what going on inside the Konsist test and Konsist API.

Two primary tools can help you comprehend the inner workings of the Konsit API -  [#evaluate-expression](debug-konsist-test.md#evaluate-expression "mention") and [#print-to-console](debug-konsist-test.md#print-to-console "mention").

## Evaluate Expression

The [IntelliJ IDEA](https://www.jetbrains.com/idea/) / [Android Studio](https://developer.android.com/studio) provides a handy feature called [Evaluate Expressions](https://www.jetbrains.com/help/rider/Evaluating\_Expressions.html#eval-expression-dialog) that is ideal for debugging Konsist tests.

Create a simple test class and click on the line number to add the breakpoint:

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

Debug Test:

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

When the program stops at the breakpoint run `Evaluate Expression` action:

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

In the `Evaluate` window enter the query and click `Evaluate` the button. For example, you can list all of the classes present in the scope to get the class names:

<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

You can also display a single-class declaration to view its `name`:

```kotlin
koScope
    .classes()
    .first()
    .name
```

## Print To Console

Konsist provides an API to print the desired data as console logs.

Print a list of files from `KoScope` :

```kotlin
koScope // KoScope
    .print()
```

Print multiple declarations:

```kotlin
koScope
    .classes() // List<KoClassDeclaration>
    .print()
```

Print a given attribute for each declaration:

```kotlin
koScope
    .classes() // List<KoClassDeclaration>
    .print { it.fullyQualifiedName }
```

Print single declaration:

```kotlin
koScope
    .classes() // List<KoClassDeclaration>
    .first() // KoClassDeclaration
    .print()
```

Print list of queried declarations before and after query:

```kotlin
koScope
    .classes() // List<KoClassDeclaration>
    .print(prefix = "Before") // or .print(prefix = "Before") { it.name }
    .withSomeAnnotations("Logger")
    .print(prefix = "After") // or .print(prefix = "After") { it.name }
```

Print nested declarations:

```kotlin
koScope
    .classes() // List<KoClassDeclaration>
    .constructors // List<KoConstructorDeclaration>
    .parameters //  List<KoParameterDeclaration>
    .print()
```



