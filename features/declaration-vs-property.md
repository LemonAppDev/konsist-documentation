# Declaration Vs Property

Some code constructs can be represented as declarations (declaration-site) and properties (use-site).&#x20;

## Declaration Site

Consider this annotation class:

```kotlin
annotation class CustomAnnotation
```

The above code represents the declaration of `CustomAnnotation` the annotation class, the place in the code where this annotation is declared (declaration-site). This declaration can be retrieved by filtering `KoScope` declarations...

```kotlin
koScope
    .declarations()
    .filterIsInstance<KoAnnotationDeclaration>()
```

..or using the `annotations` method directly:

```kotlin
koScope.annotations() // Sequence<KoAnnotationDeclaration>
```

Such declaration can be used to check if annotations declared in a given package have the correct name suffix:

```kotlin
// Every annotation declared inside com.logger package must have a name ending with "Logger"

koScope
    .annotations()
    .withPackage("com.logger")
    .assert {
        it.hasNameEndingWith("Logger")
    }
```

## Use Site

Now consider this function:

```kotlin
@CustomAnnotation
fun logHello() {
    println("Hello")
}
```

The above code also contains `CustomAnnotation` annotation. However, this time code represents the place in the code where the annotation is used (use-site). Such annotations can be accessed using the `annotations` property:

```
koFunction.annotations
```

Such properties can be used to check if the function with certain name have the correct name suffix:

```kotlin
// Every function with name starting with "log" is annotated with CustomAnnotation

koScope
    .functions()
    .withNameStartingWith("log")
    .assert {
        it.hasAnnotation("CustomAnnotation")
    }
```



