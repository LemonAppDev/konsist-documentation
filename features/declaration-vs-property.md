# Declaration Vs Property

Some code constructs can be represented as declarations (declaration-site) and as properties (use-site).

## Declaration Site

Consider this annotation class:

```kotlin
annotation class CustomLogger
```

The above code represents the declaration of the `CustomLogger` annotation class, the place in the code where this annotation is declared (declaration-site). This declaration can be retrieved by filtering `KoScope` declarations...

```kotlin
koScope
    .classes()
    .withAllModifiers(KoModifier.ANNOTATION)
```

For example, such declaration can be used to check if annotations reside in a desired package:

```kotlin
// Every annotation declared inside com.logger package must have a name ending with "Logger"

koScope
    .classes()
    .withAllModifiers(KoModifier.ANNOTATION)
    .assert { it.resideInPackage("..annotation..") }
```

## Use Site

Now consider this function:

```kotlin
@CustomLogger
fun logHello() {
    println("Hello")
}
```

The above code also contains `CustomLogger` annotation. However, this time code represents the place in the code where the annotation is used (use-site). Such annotations can be accessed using the `annotations` property:

```
koFunction.annotations
```

Such properties can be used to check if the function annotated with `CustomLogger` annotation has the correct name prefix:

```kotlin
// Every function with a name starting with "log" is annotated with CustomLogger

koScope
    .functions()
    .withAllAnnotations("CustomLogger")
    .assert {
        it.hasNameStartingWith("log")
    }
```
