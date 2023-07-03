---
description: What is declaration?
---

# Declaration

The declaration (`KoDeclaration`) represents a code entity, a piece of Kotlin code. Every parsed Kotlin File (`KoFileDeclaration`) (usually) contains multiple declarations. The declaration can be a package (`KoPackageDeclaration`), property (`KoPropertyDeclaration`), annotation (`KoAnnotationDeclaration`), class (`KoClassDeclaration`), etc.

Consider this Kotlin code snippet file:

```kotlin
private const val logLevel = "debug"

@Entity
open class Logger(val level: String) {
   fun log(message: String) {
   
   } 
}
```

The above snippet is represented by the `KoFileDeclaration`class. It contains two declarations - property declaration (`KoPropertyDeclaration`) and class declaration (`KoClassDeclaration`). The `Logger` class declaration contains a single function declaration (`KoFunctionDeclaration` ):

```mermaid
%%{init: {'theme':'forest'}}%%
flowchart TD
    KoFile
    KoFile---KoProperty
    KoFile---KoClass
    KoClass---KoFunction
```

Declarations mimic the Kotlin file structure. Konsts API provides a way to retrieve every element. To get all functions in all classes inside the file using `.classes().functions()` :

```kotlin
koFile // Sequence<KoFile>
    .classes()  // Sequence<KoClassDeclaration>
    .functions() // Sequence<KoFunctionDeclaration>
```

{% hint style="info" %}
To print declaration content use `koDeclaration.print()` method.
{% endhint %}

## Declaration Properties

Each declaration contains a set of properties to facilitate filtering and verification eg. `KoClass` declaration has `name`,  `modifiers` , `annotations` , `declarations` (containing `KoFunction`) etc. Here is how the `name` of the function can be retrieved.

```kotlin
val name = koFile // Sequence<KoFileDeclaration>
    .classes()  // Sequence<KoClassDeclaration>
    .functions() // Sequence<KoFunctionDeclaration>
    .first() // KoFunctionDeclaration
    .name // String
    
println(name) // prints: log
```

Although it is possible to retrieve a property of a single declaration usually verification is performed on a collection of declarations matching certain criteria eg. methods annotated with specific annotations or classes residing within a single package. See the [declaration-query-and-filter.md](../writing-tests/declaration-query-and-filter.md "mention") page.

## Debugging Declaration Properties

Each declaration exposes a few additional properties to help with debugging:

* `text` - provides declaration text eg. `val property role = "Developer"`
* `location` - provides file path with file name, line, and column e.g. `~\Dev\IdeaProject\SampleApp\src\kotlin\com\sample\Logger:10:5`
* `locationWithText` - provides `location` together with the declaration `text`

