# Kotlin Serialization Snippets

Konsist can be used to guard the consistency of the [Kotlin Serialization](https://kotlinlang.org/docs/serialization.html) 
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
        .assert {
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
        .assert { it.hasAnnotationOf(SerialName::class) }
}
```

