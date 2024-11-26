# Verify Classes

Konsist enables development teams to enforce structural rules for class ensuring code consistency across projects.

To verify classes start by querying all classes present in the project:

```kotlin
Konsist
.scopeFromProject()
.classes()
...
```

{% hint style="info" %}
The above code selects all classes present in the project codebase. While this demonstrates Konsist's API capabilities, in practical scenarios you'll typically want to verify a specific subset of classes - such as those with a particular name suffix or classes within a given package. See [koscope.md](../writing-tests/koscope.md "mention") and [declaration-query-and-filter.md](../writing-tests/declaration-query-and-filter.md "mention").&#x20;
{% endhint %}

Konsist allows you to verify multiple aspects of a class. For a complete understanding of the available APIs, refer to the language reference documentation for [KoClassDeclaration](https://lemonappdev.github.io/konsist/-konsist%200.17.0/com.lemonappdev.konsist.api.declaration/-ko-class-declaration/index.html).

Let's look at few examples.

## Verify Name

Class names can be validated to ensure they follow project naming conventions and patterns.

Check if class name ends with `Repository`:

```kotlin
...
.assertTrue {
   it.hasNameEndingWith("Repository")
}
```

## Verify Modifiers

Class modifiers can be validated to ensure proper encapsulation and access control.

Check if class has `internal` modifier:

```kotlin
...
.assertTrue {
   it.hasInternalModifier
}
```

## Verify Annotations

Class-level and member annotations can be verified for presence, correct usage, and required attribute values.

Check if class is annotated with `Service` annotation:

```kotlin
...
.assertTrue {
   it.hasAnnotationOf(Service::class)
}
```

## Verify Package

Package declarations can be validated to ensure classes are located in the correct package structure according to architectural guidelines.

Check if class has `model` package or sub-packages (`..` means include sub-packages):

```kotlin
...
.assertTrue {
   it.resideInPackage("com.lemonappdev.model..")
}
```

## Verify Methods

Methods can be validated for their signatures, modifiers, annotations, naming patterns, return types, and parameter structures.

Check if methods (functions defined inside class) have no annotations:

```kotlin
...
.functions()
.assertTrue {
   it.annotations.isEmpty()
}
```

See [Broken link](broken-reference "mention").

## Verify Properties

Properties can be checked for proper access modifiers, type declarations, and initialization patterns.

Check if all properties (defined inside class) has `val` modifiers:

```kotlin
...
.properties()
.assertTrue {
   it.isVal
}
```

See [#verify-properties](verify-classes.md#verify-properties "mention").

## Verify Constructors

Primary and secondary constructors can be validated for parameter count, types, and proper initialization.

Check if class has explicit primary constructor:

```kotlin
...
.assertTrue {
   it.hasPrimaryConstructor
}
```

Check if primary constructor is annotated with `Inject` annotation:

```kotlin
...
.primaryConstructors
.assertTrue {
    it.hasAnnotation(Inject::class)
}
```

## Verify Generic Type Parameters

Generic type parameters and constraints can be checked for correct usage and bounds declarations.

Check if class has not type parameters:

<pre class="language-kotlin"><code class="lang-kotlin">...
.assertFalse {
<strong>    it.hasTypeParameters()
</strong>}
</code></pre>

## Verify Generic Type Arguments

Generic type arguments can be checked for correct usage.

Check if parent has no type arguments:

```kotlin
...
.parents()
.assertFalse {
    it.hasTypeArguments()
}
```

## Verify Parents

Inheritance hierarchies, interfaces implementations, and superclass relationships can be validated.

Check if class extends `CrudRepository`:

```kotlin
...
.assertTrue {
   it.hasParentOf(CrudRepository::class)
}
```

## Verify Companion Objects

Companion object declarations, their contents, and usage patterns can be verified for compliance.

Check if class has companion object:

```kotlin
...
.assertTrue { declaration ->
    declaration.hasObject { it.hasCompanionModifier }
}
```

## Verify Members Order

The sequential arrangement of class members can be enforced according to defined organizational rules.

Check if class properties are defined before functions:

```kotlin
...
.assertTrue {
    val lastKoPropertyDeclarationIndex = it
        .declarations(includeNested = false, includeLocal = false)
        .indexOfLastInstance<KoPropertyDeclaration>()
    
    val firstKoFunctionDeclarationIndex = it
        .declarations(includeNested = false, includeLocal = false)
        .indexOfFirstInstance<KoFunctionDeclaration>()
    
    if (lastKoPropertyDeclarationIndex != -1 && firstKoFunctionDeclarationIndex != -1) {
        lastKoPropertyDeclarationIndex < firstKoFunctionDeclarationIndex
    } else {
        true
    }
}
```





