# Verify Interfaces

Konsist enables development teams to enforce structural rules for interfaces ensuring code consistency across projects.

To verify interfaces start by querying all interface present in the project:

```kotlin
Konsist
.scopeFromProject()
.interfaces()
...
```

{% hint style="info" %}
The above code selects all interfaces present in the project codebase. While this demonstrates Konsist's API capabilities, in practical scenarios you'll typically want to verify a specific subset of interface - such as those with a particular name suffix or interfaces within a given package. See [koscope.md](../writing-tests/koscope.md "mention") and [declaration-query-and-filter.md](../writing-tests/declaration-query-and-filter.md "mention").&#x20;
{% endhint %}

Konsist allows you to verify multiple aspects of a interfaces. For a complete understanding of the available APIs, refer to the language reference documentation for [KoInterfaceDeclaration](https://lemonappdev.github.io/konsist/-konsist%200.17.0/com.lemonappdev.konsist.api.declaration/-ko-interface-declaration/index.html).

Let's look at few examples.

## Verify Name

Interface names can be validated to ensure they follow project naming conventions and patterns.

Check if interface name ends with `Repository`:

```kotlin
..
.assertTrue {
   it.hasNameEndingWith("Repository")
}
```

## Verify Modifiers

Interface modifiers can be validated to ensure proper encapsulation and access control.

Check if interface has `internal` modifier:

```kotlin
..
.assertTrue {
   it.hasInternalModifier
}
```

## Verify Annotations

Interface-level and member annotations can be verified for presence, correct usage, and required attribute values.

Check if interface is annotated with `Service` annotation:

```kotlin
...
.assertTrue {
   it.hasAnnotationOf(Service::class)
}
```

## Verify Package

Package declarations can be validated to ensure classes are located in the correct package structure according to architectural guidelines.

Check if interface has `model` package or sub-packages (`..` means include sub-packages):

```kotlin
...
.properties()
.assertTrue {
   it.resideInPackage("com.lemonappdev.model..")
}
```

## Verify Methods

Methods can be validated for their signatures, modifiers, annotations, naming patterns, return types, and parameter structures.

Check if methods (functions defined inside interface) have name starting with `Local`:

```kotlin
...
.functions()
.assertTrue {
    it.all { function -> function.hasNameStartingWith("Local") }
}
```

See [Broken link](broken-reference "mention").

## Verify Properties

Properties can be checked for proper access modifiers, type declarations, and initialization patterns.

Check if all properties (defined inside interface) has `val` modifiers:

```kotlin
...
.properties()
.assertTrue {
   it.all { property -> property.isVal }
}
```

See [#verify-properties](verify-interfaces.md#verify-properties "mention").

## Verify Generic Type Parameters

Generic type parameters and constraints can be checked for correct usage and bounds declarations.

Check if interface has not type parameters:

<pre class="language-kotlin"><code class="lang-kotlin">...
.assertTrue {
<strong>    it.typeParameters.isEmpty()
</strong>}
</code></pre>

## Verify Parents

Inheritance hierarchies, interfaces implementations, and superclass relationships can be validated.

Check if interface extends `CrudRepository`:

```kotlin
...
.assertTrue {
   it.hasParentOf(Class::CrudRepository)
}
```

## Verify Companion Objects

Companion object declarations, their contents, and usage patterns can be verified for compliance.

Check if interface has `companion object`:

```kotlin
...
.assertTrue {
   it.objects().any { objectt -> objectt.hasCompanionModifier }
}
```

## Verify Members Order

The sequential arrangement of interface members can be enforced according to defined organizational rules.

Check if interface properties are defined before functions:

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





