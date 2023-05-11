# Compiler Inference

The primary focus of Konsist API is to reflect the state of the Kotlin source code (that will be verified), not the state of the running program. The Kotlin compiler has a deeper understanding of the Kotlin code base than Konsist, because the compiler can infer more information. Let's take a look at a few examples.

### Class Example

Consider this class:&#x20;

```kotlin
class Logger
```

Is `Logger` class `public`? Yes obviously it is `public`, however, the `hasPublicModifier` method returns the `false` value:

```
koClass.hasPublicModifier() // false
```

Why is that? The `public` visibility modifier is the default visibility modifier in Kotlin. Meaning that class will be `public` even if it does not have the explicit `public` modifier. Since the class has no `public` modifier the `hasPublicModifier` method returns false. To distinguish between class being `public` and class heaving explicit`public` modifier Konsist API provides another method to retrieve declaration visibility:

```
koClass.isPublicOrDefault() // true
```

### Primary Constructor Example

Let's look at the primary constructor for the same class:

```kotlin
class Logger
```

The `Logger` the class has a primary constructor because the Kotlin compiler will generate a parameterless constrictor under the hood. However, the Konsist API will return a `null` value  because the primary constructor is not present in the Kotlin source code:

```
koClass.primaryConstructor // null
```

### Function Return Type Example

Consider this function:

```
fun getName() = "Konsist"
```

Kotlin will infer `String` as the return type of the `getName` function. Since the source code does not contain this explicit return type Konsist lacks information about the return type.  In this scenario, `hasReturnType` the method will return the `false` value:

```kotlin
koFunction.hasReturnType() // false
```

Unlike the previous example, Konsist has no way to determine the actual function return type.
