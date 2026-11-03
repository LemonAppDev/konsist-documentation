# Verify Functions

Functions can be validated for their signatures, modifiers, naming patterns, return types, and parameter structures.

To verify functions start by querying all functions present in the project:

```kotlin
Konsist
.scopeFromProject()
.functions()
...
```

In practical scenarios you'll typically want to verify a specific subset of functions - such as those defined inside classes:

```kotlin
Konsist
.scopeFromProject()
.classes()
.functions()
...
```

Local functions are included by default. To exclude them, set `includeLocal = false`:

```kotlin
Konsist
.scopeFromProject()
.classes()
.functions(includeLocal = false)
...
```

Konsist allows you to verify multiple aspects of functions. For the complete list of available APIs, explore `KoFunctionDeclaration` in your IDE (code completion and KDoc).

Let's look at a few examples.

## Verify Name

Function names can be validated to ensure they follow project naming conventions and patterns.

Check if function name starts with `get`:

```kotlin
...
.assertTrue {
   it.hasNameStartingWith("get")
}
```

## Verify Modifiers

Function modifiers can be validated to ensure proper encapsulation and access control.

Check if function has `public` or default (also `public`) modifier:

```kotlin
...
.assertTrue {
   it.hasPublicOrDefaultModifier
}
```

## Verify Annotations

Function-level and member annotations can be verified for presence, correct usage, and required attribute values.

Check if function is annotated with `Binding` annotation:

```kotlin
...
.assertTrue {
   it.hasAnnotationOf(Binding::class)
}
```

## **Verify Body Type**

Functions with block bodies (using curly braces) can be validated to ensure compliance with code structure requirements:

<pre class="language-kotlin"><code class="lang-kotlin">...
<strong>.assertTrue { 
</strong>    it.hasBlockBody 
}
</code></pre>

Expression body functions (using single-expression syntax) can be verified to maintain consistent style across the codebase:

```kotlin
...
.assertTrue { 
    it.hasExpressionBody 
}
```

Function body content can be verified using `body` property. It contains the block body without the enclosing braces (or the expression for an expression body), and is `null` when function has no body:

```kotlin
...
.assertFalse { 
    it.body?.contains("println(") == true
}
```

## **Verify Parameters**

Function parameters can be validated for their types, names, modifiers, and annotations to ensure consistent parameter usage.

Check if function has parameter of type `String`:

```kotlin
...
.assertTrue { 
    it.hasParameter { parameter  -> parameter.hasTypeOf(String::class) }
}
```

## **Verify Context Parameters**

Function [context parameters](https://kotlinlang.org/docs/context-parameters.html) can be verified:

```kotlin
// Code Snippet
context(logger: Logger)
fun saveOrder(order: Order) { }
```

Check if function has `Logger` context parameter:

```kotlin
...
.assertTrue { 
    it.hasContextParameter { contextParameter -> contextParameter.hasTypeOf(Logger::class) }
}
```

Check if function has context parameter named `logger`:

```kotlin
...
.assertTrue { 
    it.hasContextParameterWithName("logger")
}
```

Context parameters can be accessed using `contextParameters` property (anonymous context parameter `context(_: Logger)` has `_` name) and filtered using `withContextParameters()`, `withContextParameterNamed(...)`, `withContextParameter { }` and their `without...` counterparts.

## **Verify Return Type**

Return types can be checked to ensure functions follow expected return type patterns and contracts.

Check if function return type is a Kotlin collection type:

```kotlin
...
.assertTrue { 
    it.returnType?.sourceDeclaration?.isKotlinCollectionType
}
```

## **Verify Generic Parameters**

Generic type parameters can be validated to ensure proper generic type usage and constraints.

Check if function has type parameters:

```kotlin
...
.assertTrue { 
    it.hasTypeParameters()
}
```

## Verify Generic Type Arguments

Generic type arguments can be checked for correct usage.

Check if return type has no type arguments:

```kotlin
...
.assertFalse {
    it.returnType?.hasTypeArguments()
}
```

## **Verify Test Class**

Function test classes can be verified using a predicate. The predicate is evaluated against classes from the test source sets (optionally limited by `moduleName` and `sourceSetName` parameters).

Check if function has a test class with name containing the function name:

```kotlin
...
.assertTrue { function ->
    function.hasTestClass { testClass -> 
        testClass.hasNameContaining(function.name, ignoreCase = true) 
    }
}
```

Matching test classes can be retrieved using `testClasses { }` and counted using `countTestClasses { }`. Function lists can be filtered using `withTestClass { }` and `withoutTestClass { }`.

## **Verify Top Level**

Top-level functions (functions not declared inside a class) can be specifically queried and validated:

```kotlin
...
.assertTrue { 
    it.isTopLevel
}
```

This helps ensure top-level functions follow project conventions, such as limiting their usage or enforcing specific naming patterns.
