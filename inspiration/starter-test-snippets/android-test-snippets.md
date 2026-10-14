# Android Test Snippets

Konsist can be used to guard the consistency of the [Android](https://www.android.com/) project.

{% hint style="info" %}
The [android-showcase](https://github.com/igorwojda/android-showcase) project contains a set of Konsist tests.
{% endhint %}

## 1. Classes Extending `ViewModel` Should Have `ViewModel` Suffix

```kotlin
@Test
fun `classes extending 'ViewModel' should have 'ViewModel' suffix`() {
    Konsist
        .scopeFromProject()
        .classes()
        .withParentClassOf(ViewModel::class)
        .assertTrue { it.name.endsWith("ViewModel") }
}
```

## 2. Every `ViewModel` Public Property Has `Flow` Type

```kotlin
@Test
fun `Every 'ViewModel' public property has 'Flow' type`() {
    Konsist
        .scopeFromProject()
        .classes()
        .withParentClassOf(ViewModel::class)
        .properties()
        .withPublicOrDefaultModifier()
        .assertTrue {
            it.hasType { type -> type.bareSourceType == "Flow" }
        }
}
```

## 3. Every `ViewModel` `StateFlow` Property Has `MutableStateFlow` Explicit Backing Field

```kotlin
@Test
fun `Every 'ViewModel' 'StateFlow' property has 'MutableStateFlow' explicit backing field`() {
    Konsist
        .scopeFromProject()
        .classes()
        .withParentClassOf(ViewModel::class)
        .properties()
        .withTypeOf(StateFlow::class)
        .assertTrue {
            it.hasExplicitBackingField { field -> field.hasTacitTypeOf(MutableStateFlow::class) }
        }
}
```

## 4. `Repository` Classes Should Reside In `repository` Package

```kotlin
@Test
fun `'Repository' classes should reside in 'repository' package`() {
    Konsist
        .scopeFromProject()
        .classes()
        .withNameEndingWith("Repository")
        .assertTrue { it.residesInPackage("..repository..") }
}
```

## 5. No Class Should Use Android Util Logging

```kotlin
@Test
fun `no class should use Android util logging`() {
    Konsist
        .scopeFromProject()
        .files
        .assertFalse { it.hasImport { import -> import.name == "android.util.Log" } }
}
```

## 6. All Jetpack Compose Previews Contain `Preview` In Method Name

```kotlin
@Test
fun `All Jetpack Compose previews contain 'Preview' in method name`() {
    Konsist
        .scopeFromProject()
        .functions()
        .withAnnotationOf(Preview::class)
        .assertTrue {
            it.hasNameContaining("Preview")
        }
}
```

## 7. Every Class With Serializable Must Have Its Properties Serializable

```kotlin
@Test
fun `every class with Serializable must have its properties Serializable`() {
    val message =
        """In Android, every serializable class must implement the Serializable interface 
    |or be a simple non-enum type because this is how the Java and Android serialization 
    |mechanisms identify which objects can be safely converted to a byte stream for 
    |storage or transmission, ensuring that complex objects can be properly reconstructed 
    |when deserialized.""".trimMargin()

    Konsist
        .scopeFromProduction()
        .classes()
        .withParentNamed("Serializable")
        .properties()
        .types
        .sourceDeclarations()
        .withoutKotlinBasicTypeDeclaration()
        .withoutClassDeclaration { it.hasEnumModifier }
        .assertTrue(additionalMessage = message) {
            it.asClassDeclaration()?.hasParentWithName("Serializable")
        }
}
```

