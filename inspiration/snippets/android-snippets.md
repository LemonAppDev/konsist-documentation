# Android Snippets

Konsist can be used to guard the consistency of the [Android](https://www.android.com/) project.&#x20;

{% hint style="info" %}
The [android-showcase](https://github.com/igorwojda/android-showcase) project contains set of kotnsist tests.
{% endhint %}
## Snippet : 1Classes Extending 'ViewModel' Should Have 'ViewModel' Suffix

```kotlin
@Test
fun `classes extending 'ViewModel' should have 'ViewModel' suffix`() {
    Konsist
        .scopeFromProject()
        .classes()
        .withParentClassOf(ViewModel::class)
        .assert { it.name.endsWith("ViewModel") }
}
```

## Snippet : 1No Class Should Use Android Util Logging

```kotlin
@Test
fun `no class should use Android util logging`() {
    Konsist
        .scopeFromProject()
        .files
        .assertNot { it.hasImports("android.util.Log") }
}
```

