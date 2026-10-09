# Verify Generics

## Type Parameter vs Type Argument

To understand the Konsist API, let's look at the difference between `generic type parameters` and `generic type arguments`:

1. **Type Parameter** is the placeholder (like `T`) you write when _creating_ a class or function (declaration site)
2. **Type Argument** is the actual type (like `String` or `Int`) you provide when _using_ that class or function (use site)

Simple Examples:

```kotlin
// Example 1: Class
// Here 'T' is a TYPE PARAMETER
class Box<T>(val item: T)

// Here 'String' is a TYPE ARGUMENT
val stringBox = Box<String>("Hello")


// Example 2: Function
// Here 'T' is a TYPE PARAMETER
fun <T> printWithType(item: T) {
    println("Type is: ${item::class.simpleName}")
}

// Here 'String' is a TYPE ARGUMENT
printWithType<String>("Hello")  // prints: Type is: String
```

## Verify Type Parameters

Type parameters can be defined, for example, inside class or function.

### Check whether a class's generic type parameter has the name `UiState`:

```kotlin
// Code Snippet 
class View<UiState>(val state: UiState) // UiState is a type parameter

// Konsist
Konsist
    .scopeFromProject()
    .classes()
    .typeParameters // access type parameters
    .assertTrue {
        it.name == "UiState" // true
    }
```

### Check whether class `type parameters` has `out` modifier:

```kotlin
// Code Snippet 
class Producer<out T>(val item: T)

// Konsist
Konsist
    .scopeFromProject()
    .classes()
    .typeParameters // access type parameters
    .assertTrue {
        it.hasOutModifier // true
    }
```

## Verify Type Arguments

### Check whether a property generic type argument has the name `Service`:

```kotlin
//Code Snippet 
val services: List<Service> = emptyList()

// Konsist
Konsist
    .scopeFromProject()
    .properties()
    .assertTrue { property ->
        property
            .type
            ?.typeArguments
            ?.flatten()
            ?.any { typeArgument -> typeArgument.name == "Service" }
    }
```

The `flatten()` extension method allows flattening the type arguments structure:

* For a type like `String`, it returns `listOf()`.
* For a type like `List<String>`, it returns `listOf(String)`.
* For a type like `Map<List<String>, Int>`, it returns `listOf(List, String, Int)`.

### Check if all function parameters have generic type argument ending with `UIState`:

```kotlin
// Snippet 
fun setState(uiState: View<WelcomeUIState>)

// Konsist Test
Konsist
    .scopeFromProject()
    .functions()
    .parameters
    .types
    .typeArguments
    .assertTrue { 
        it.hasNameEndingWith("UIState")  // true
    }
```

### Check all parents have `String` type argument:

```kotlin
// Snippet 
open class Container<T>(private val item: T) { }
class StringContainer(text: String) : Container<String>(text) { }

// Konsist Test
Konsist
    .scopeFromProject()
    .classes()
    .parents()
    .typeArguments
    .flatten()
    .assertTrue { 
        it.name == "String"  // true
    }
```
