# Verify Source Declarations

The source declaration (`sourceDeclaration` property) holds the reference to the actual type declaration such as class or interface.

Konsist API allows verifying properties of such types, e.g.:

* Check if property type implements certain interface
* Check if function return type name ends with `Repository`
* Check if parent class is annotated with given annotation

Every type used by a declaration (e.g. property type, function return type, parent) exposes the `sourceDeclaration` property.

Let's look at a few examples:

## Verify Property Source Declaration

Check if the `current` property has a type which is a class declaration having the `internal` modifier:

```kotlin
// Code Snippet
internal class Engine
val current: Engine? = null

// Konsist test
Konsist
   .scopeFromProject()
   .properties()
   .assertTrue {
      it
      .type
      ?.sourceDeclaration
      ?.asClassDeclaration()
      ?.hasInternalModifier // true
   }

```

{% hint style="info" %}
Note that explicit casting (`asXDeclaration`) has to be used to access specific properties of the declaration.
{% endhint %}

## Verify Function Return Type Source Declaration

Check if function return type is a basic Kotlin type:

```kotlin
// Code Snippet
internal class Engine {
   fun start(): Boolean = true
}

// Konsist test
Konsist
   .scopeFromProject()
   .classes()
   .functions()
   .assertTrue {
      it.returnType
      ?.sourceDeclaration
      ?.isKotlinBasicType
   }
```

## Verify Class Has Interface Source Declaration

Check if all class parents are interfaces:

```kotlin
// Code Snippet
interface Vehicle

internal class Engine : Vehicle

// Konsist test
Konsist
   .scopeFromProject()
   .classes()
   .parents()
   .assertTrue {
      it
      .sourceDeclaration
      ?.isInterface
   }
```

