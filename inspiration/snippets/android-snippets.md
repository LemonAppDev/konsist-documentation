#  Android Snippets

## Classes Extending 'ViewModel' Should Have 'ViewModel' Suffix

```kotlin
@Test
fun `classes extending 'ViewModel' should have 'ViewModel' suffix`() {
        Konsist.scopeFromProject()
            .classes()
            .withParentClassOf(ViewModel::class)
            .assert { it.name.endsWith("ViewModel") }
    }

    ```

## No Class Should Use Android Util Logging

```kotlin
@Test
fun `no class should use Android util logging`() {
        Konsist.scopeFromProject()
            .files
            .assertNot { it.hasImports("android.util.Log") }
    }
```

