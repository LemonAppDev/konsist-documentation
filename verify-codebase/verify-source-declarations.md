# Verify Source Declarations

The source declaration (`sourceDeclaration` property) holds the reference to actual type declaration such as class or interface.

Konsist API allows for verify properties of such type e.g.:

* Check if property type implements certain interface
* Check if function return type name ends with `Repository`
* Check if parent class is annotated with given annotation

Every declaration that is using another type such as property, function, parent exposes `sourceDeclaration` property.

Let's look at few examples:

## Verify Property Source Declaration

Check if property type implements certain interface:

```kotlin
// Code Snippet
internal class Engine
val current: Engine? = null // testing this in below test

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

Check if type of `current` property is has a type which is a class declaration heaving `internal` modifier:

```kotlin
// Code Snippet
internal class Engine
val current: Engine? = null // testing this in below test

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
   fun start(): Boolean
}

// Konsist test
Konsist
   .scopeFromProject()
   .classes()
   .functions()
   .assertTrue {
      it.returnType?
      .sourceDeclaration
      ?.isKotlinBasicType
   }
```

## Verify Class Has Interface Source Declaration

```kotlin
// Code Snippet
internal class Engine {
   fun start(): Boolean
}

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

