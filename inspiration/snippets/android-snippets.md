# Android Snippets

Konsist can be used to guard the consistency of the [Android](https://www.android.com/) project.

{% hint style="info" %}
The [android-showcase](https://github.com/igorwojda/android-showcase) project contains set of Konsist tests.
{% endhint %}

## 1. C l a s s e s   E x t e n d i n g   ` V i e w M o d e l `   S h o u l d   H a v e   ` V i e w M o d e l `   S u f f i x

```kotlin
@Test
fun `classes extending 'ViewModel' should have 'ViewModel' suffix`() {
    Konsist
        .scopeFromProject()
        .classes()
        .withParentOf(ViewModel::class)
        .assertTrue { it.name.endsWith("ViewModel") }
}
```

## 2. E v e r y   ` V i e w M o d e l `   P u b l i c   P r o p e r t y   H a s   ` F l o w `   T y p e

```kotlin
@Test
fun `Every 'ViewModel' public property has 'Flow' type`() {
    Konsist
        .scopeFromProject()
        .classes()
        .withParentOf(ViewModel::class)
        .properties()
        .assertTrue {
            it.hasPublicOrDefaultModifier && it.hasType("kotlinx.coroutines.flow.Flow")
        }
}
```

## 3. ` R e p o s i t o r y `   C l a s s e s   S h o u l d   R e s i d e   I n   ` r e p o s i t o r y `   P a c k a g e

```kotlin
@Test
fun `'Repository' classes should reside in 'repository' package`() {
    Konsist
        .scopeFromProject()
        .classes()
        .withNameEndingWith("Repository")
        .assertTrue { it.resideInPackage("..repository..") }
}
```

## 4. N o   C l a s s   S h o u l d   U s e   A n d r o i d   U t i l   L o g g i n g

```kotlin
@Test
fun `no class should use Android util logging`() {
    Konsist
        .scopeFromProject()
        .files
        .assertFalse { it.hasImport { import -> import.name == "android.util.Log" } }
}
```

