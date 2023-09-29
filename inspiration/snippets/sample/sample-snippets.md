# Sample Snippets

Teeeest


## 1. Classes Extending `ViewModel` Should Have `ViewModel` Suffix

```kotlin
@Test
fun `classes extending 'ViewModel' should have 'ViewModel' suffix`() {
    Konsist
        .scopeFromProject()
        .classes()
        .withParentOf(ViewModel::class)
        .assert { it.name.endsWith("ViewModel") }
}
```

## 2. Every `ViewModel` Public Property Has `Flow` Type

```kotlin
@Test
fun `Every 'ViewModel' public property has 'Flow' type`() {
    Konsist
        .scopeFromProject()
        .classes()
        .withParentOf(ViewModel::class)
        .properties()
        .assert {
            it.hasPublicOrDefaultModifier && it.hasType { type -> type.name == "kotlinx.coroutines.flow.Flow" }
        }
}
```

## 3. `Repository` Classes Should Reside In `repository` Package

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

## 4. No Class Should Use Android Util Logging

```kotlin
@Test
fun `no class should use Android util logging`() {
    Konsist
        .scopeFromProject()
        .files
        .assertNot { it.hasImport { import -> import.name == "android.util.Log" } }
}
```

