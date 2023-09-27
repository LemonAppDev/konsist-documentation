# Library Snippets

## 1. E v e r y   A p i   D e c l a r a t i o n   H a s   K D o c

```kotlin
@Test
fun `every api declaration has KDoc`() {
    Konsist
        .scopeFromPackage("..api..")
        .declarationsOf<KoKDocProvider>()
        .assertTrue { it.hasKDoc }
}
```

## 2. E v e r y   F u n c t i o n   W i t h   P a r a m e t e r s   H a s   A   P a r a m   T a g s

```kotlin
@Test
fun `every function with parameters has a param tags`() {
    Konsist.scopeFromPackage("..api..")
        .functions()
        .assertTrue { it.hasValidKDocParamTags() }
}
```

## 3. E v e r y   F u n c t i o n   W i t h   R e t u r n   V a l u e   H a s   A   R e t u r n   T a g

```kotlin
@Test
fun `every function with return value has a return tag`() {
    Konsist.scopeFromPackage("..api..")
        .functions()
        .assertTrue { it.hasValidKDocReturnTag() }
}
```

## 4. E v e r y   E x t e n s i o n   H a s   A   R e c e i v e r   T a g

```kotlin
@Test
fun `every extension has a receiver tag`() {
    Konsist.scopeFromPackage("..api..")
        .declarationsOf<KoReceiverTypeProvider>()
        .assertTrue { it.hasValidKDocReceiverTag() }
}
```

## 5. E v e r y   P u b l i c   F u n c t i o n   I n   A p i   P a c k a g e   M u s t   H a v e   E x p l i c i t   R e t u r n   T y p e

```kotlin
@Test
fun `every public function in api package must have explicit return type`() {
    Konsist
        .scopeFromPackage("..api..")
        .functions()
        .assertTrue { it.hasReturnType() }
}
```

## 6. E v e r y   P u b l i c   P r o p e r t y   I n   A p i   P a c k a g e   M u s t   H a v e   S p e c i f y   T y p e   E x p l i c i t l y

```kotlin
@Test
fun `every public property in api package must have specify type explicitly`() {
    Konsist
        .scopeFromPackage("..api..")
        .properties()
        .assertTrue { it.hasType() }
}
```

