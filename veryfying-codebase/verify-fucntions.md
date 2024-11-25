# Verify Fucntions

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

Konsist API allows to query `local` functions:

```kotlin
Konsist
.scopeFromProject()
.classes()
.functions(includeLocal = true)
...
```

Konsist allows you to verify multiple aspects of a functions. For a complete understanding of the available APIs, refer to the language reference documentation for [KoFunctionDeclaration](https://lemonappdev.github.io/konsist/-konsist%200.17.0/com.lemonappdev.konsist.api.declaration/-ko-function-declaration/index.html).

Let's look at few examples.

## Verify Name

Function names can be validated to ensure they follow project naming conventions and patterns.

Check if function name starts with `get` :

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
..
.assertTrue {
   it.hasPublicOrDefaultModifier
}
```

## Verify Annotations

Function-level and member annotations can be verified for presence, correct usage, and required attribute values.

Check if function is annotated with `Service` annotation:

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

## **Verify Parameters**

Function parameters can be validated for their types, names, modifiers, and annotations to ensure consistent parameter usage.

Check if function has parameter of type `String`:

```kotlin
...
.assertTrue { 
    it.parameters.any { parameter  -> parameter.hasTypeOf<String>() }
}
```

## **Verify Return Type**

Return types can be checked to ensure functions follow expected return type patterns and contracts.

Check if function has Kotlin collection type:

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
    it.typeParameters.isNotEmpty()
}
```

## **Verify Top Level**

Top-level functions (functions not declared inside a class) can be specifically queried and validated:

```kotlin
...
.assertTrue { 
    it.isTopLevel
}
```

This helps ensure top-level functions follow project conventions, such as limiting their usage or enforcing specific naming patterns.











##
