# Kotlin Serialization Snippets

Konsist can be used to guard the consistency of the [Kotlin Serialization](https://kotlinlang.org/docs/serialization.html) 
library.

## 1. C l a s s e s   A n n o t a t e d   W i t h   ` S e r i a l i z a b l e `   H a v e   A l l   P r o p e r t i e s   A n n o t a t e d   W i t h   ` S e r i a l N a m e `

```kotlin
@Test
fun `classes annotated with 'Serializable' have all properties annotated with 'SerialName'`() {
    Konsist
        .scopeFromProject()
        .classes()
        .withAnnotationOf(Serializable::class)
        .properties()
        .assertTrue {
            it.hasAnnotationsOf(SerialName::class)
        }
}
```

## 2. E n u m   C l a s s e s   A n n o t a t e d   W i t h   ` S e r i a l i z a b l e `   H a v e   A l l   E n u m   C o n s t a n t s   A n n o t a t e d   W i t h   ` S e r i a l N a m e `

```kotlin
@Test
fun `enum classes annotated with 'Serializable' have all enum constants annotated with 'SerialName'`() {
    Konsist.scopeFromProject()
        .classes()
        .withEnumModifier()
        .withAnnotationOf(Serializable::class)
        .enumConstants
        .assertTrue { it.hasAnnotationsOf(SerialName::class) }
}
```

