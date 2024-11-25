# Verifying Source Declaration

The source declaration (`sourceDeclaration` property) holds the reference to actual type (declaration).&#x20;

Konsist API allows for verify properties of such type e.g.:

* Check if property type implements certain interface
* Check if function return type name ends with `Repository`
* Check if parent class is annotated with given annotation



Every declaration such as property, function, parent exposes `sourceDeclaration` property.&#x20;

Let's look at few example to check if type of `current` property is has a type which is a class declaration with `internal` modifier:

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

> Note that explicit casting (`asXDeclaration`) has to be used to access specific properties of the declaration.

