# Kotlin Serialization Snippets

Konsist can be used to guard the consistency of classes related to the [Kotlin Serialization](https://kotlinlang.
org/docs/serialization.html) 
library.

## 1. Classes Annotated With `Serializable` Have All Properties Annotated With `SerialName`

```kotlin
@Test
fun `classes annotated with 'Serializable' have all properties annotated with 'SerialName'`() {
    Konsist
        .scopeFromProject()
        .classes()
        .withAnnotationOf(Serializable::class)
        .properties()
        .assertTrue {
            it.hasAnnotationOf(SerialName::class)
        }
}
```

## 2. Enum Classes Annotated With `Serializable` Have All Enum Constants Annotated With `SerialName`

```kotlin
@Test
fun `enum classes annotated with 'Serializable' have all enum constants annotated with 'SerialName'`() {
    Konsist.scopeFromProject()
        .classes()
        .withEnumModifier()
        .withAnnotationOf(Serializable::class)
        .enumConstants
        .assertTrue { it.hasAnnotationOf(SerialName::class) }
}
```

## 3. All Models Are Serializable

```kotlin
@Test
fun `all models are serializable`() {
    Konsist
        .scopeFromPackage("com.myapp.model..")
        .classes()
        .assertTrue {
            it.hasAnnotationOf(Serializable::class)
        }
}
```

