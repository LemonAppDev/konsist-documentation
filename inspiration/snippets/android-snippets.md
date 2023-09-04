# Android Snippets

Konsist can be used to guard the consistency of the [Android](https://www.android.com/) project.

{% hint style="info" %}
The [android-showcase](https://github.com/igorwojda/android-showcase) project contains set of Konsist tests.
{% endhint %}

## Snippet 1: Classes Extending `ViewModel` Should Have `ViewModel` Suffix

```kotlin
@Test
fun `classes extending 'ViewModel' should have 'ViewModel' suffix`() {
    Konsist
        .scopeFromProject()
        .classes()
        .withAllParentsOf(ViewModel::class)
        .assert { it.name.endsWith("ViewModel") }
}
```

## Snippet 2: Every `ViewModel` public property has `Flow` type

```kotlin
@Test
fun `'ViewModel' should expose data using Kotlin 'Flow'`() {
    Konsist
        .scopeFromProject()
        .classes()
        .withAllParentsOf(ViewModel::class)
        .properties()
        .assert { 
            it.hasPublicOrDefaultModifier && it.hasType("kotlinx.coroutines.flow.Flow")
        }
}
```

## Snippet 3: No Class Should Use Android Util Logging

```kotlin
@Test
fun `'Repository' classes should reside in 'repository' package`() {
    Konsist
        .scopeFromProject()
        .classes()
        .withNameEndingWith("Repository")
        .assert { it.resideInPackage("..repository..") }
}
```

## Snippet 4: No Class Should Use Android Util Logging

```kotlin
@Test
fun `no class should use Android util logging`() {
    Konsist
        .scopeFromProject()
        .files
        .assertNot { it.hasImports("android.util.Log") }
}
```
