# Android Snippets

Konsist can be used to guard the consistency of the [Android](https://www.android.com/) project.&#x20;

{% hint style="info" %}
The [android-showcase](https://github.com/igorwojda/android-showcase) project contains set of kotnsist tests.
{% endhint %}

## Snippet 1

```kotlin
fun `classes extending 'ViewModel' should have 'ViewModel' suffix`() {
    Konsist.scopeFromProject()
        .classes()
        .withParentClassOf(ViewModel::class)
        .assert { it.name.endsWith("ViewModel") }
}
```

## Snippet 2

```kotlin
fun `no class should use Android util logging`() {
    Konsist.scopeFromProject()
        .files
        .assertNot { it.hasImports("android.util.Log") }
}
```
