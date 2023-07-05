# Android Snippets

Konsist can be used to guard the consistency of the [Android](https://www.android.com/) project.

## Snippet 1

```kotlin
fun `classes extending 'ViewModel' should have 'ViewModel' suffix`() {
    Konsist.scopeFromProject()
        .classes()
        .withParentClassOf<ViewModel>()
        .assert { it.resideInPackage("..controller..") }
}
```
