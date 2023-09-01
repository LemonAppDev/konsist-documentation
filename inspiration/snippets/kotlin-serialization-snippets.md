# Kotlin Serialization Snippets

Konsist can be used to guard the consistency of the [Kotlin Serialization](https://kotlinlang.org/docs/serialization.html) 
library.

## Snippet 1: Classes Annotated With Serializable Have All Properties Annotated With SerialName

```kotlin
@Test
fun `classes annotated with Serializable have all properties annotated with SerialName`() {
    Konsist
        .scopeFromProject()
        .classes()
        .withSomeAnnotationsOf(Serializable::class)
        .properties()
        .assert {
            it.hasAnnotationsOf(SerialName::class)
        }
}
```

## Snippet 2: Enum Classes Annotated With Serializable Have All Enum Constants Annotated With SerialName

```kotlin
@Test
fun `enum classes annotated with Serializable have all enum constants annotated with SerialName`() {
    Konsist.scopeFromProject()
        .classes()
        .withEnumModifier()
        .withSomeAnnotationsOf(Serializable::class)
        .enumConstants
        .assert { it.hasAnnotationsOf(SerialName::class) }
}
```
