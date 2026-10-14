# Verify Properties

Properties can be checked for proper access modifiers, type declarations, and initialization patterns.

To verify properties start by querying all properties present in the project:

```kotlin
Konsist
.scopeFromProject()
.properties()
...
```

In practical scenarios you'll typically want to verify a specific subset of properties - such as those defined inside classes:

```kotlin
Konsist
.scopeFromProject()
.classes()
.properties()
...
```

Konsist allows you to verify multiple aspects of properties. For the complete list of available APIs, explore `KoPropertyDeclaration` in your IDE (code completion and KDoc).

Let's look at a few examples.

## **Verify Name**&#x20;

Property names can be validated to ensure they follow project naming conventions and patterns.

Check if `Boolean` property has name starting with `is`:

```kotlin
...
.filter { it.type?.name == "Boolean" }
.assertTrue { 
    it.hasNameStartingWith("is")
}
```

## **Verify Type**&#x20;

Property types can be validated to ensure type safety and conventions:

```kotlin
...
.assertTrue { 
    it.type?.name == "LocalDateTime"
}
```

## **Verify Modifiers**&#x20;

Property modifiers can be validated to ensure proper encapsulation:

```kotlin
...
.assertTrue { 
    it.hasLateinitModifier
}
```

## **Verify Annotations**&#x20;

Property annotations can be verified for presence and correct usage:

```kotlin
...
.assertTrue { 
    it.hasAnnotationOf(JsonProperty::class)
}
```

## **Verify Accessors**&#x20;

Getter and setter presence and implementation can be validated:

Check if property has `getter`:

```kotlin
...
.assertTrue { 
    it.hasGetter
}
```

Check if property has `setter`:

```kotlin
...
.assertTrue { 
    it.hasSetter
}
```

## **Verify Initialization**&#x20;

Property initialization can be verified:

```kotlin
...
.assertTrue { 
    it.isInitialized
}
```

## **Verify Delegates**

Property delegates can be verified. Built-in delegate checks are available since Konsist `0.19.0`.

Check if property has `lazy` delegate (matches `lazy { }`, `lazy<T> { }` and `lazy(mode) { }`):

```kotlin
...
.assertTrue { 
    it.hasLazyDelegate() 
}
```

Check if property has `lazy` delegate with a given thread safety mode (`lazy` without an explicit mode uses `LazyThreadSafetyMode.SYNCHRONIZED`):

```kotlin
...
.assertTrue { 
    it.hasLazyDelegate(LazyThreadSafetyMode.NONE) 
}
```

Check if property does not use `Delegates.observable` or `Delegates.vetoable` delegate:

```kotlin
Konsist
    .scopeFromProject()
    .properties()
    .withDelegate()
    .assertFalse { 
        it.hasObservableDelegate || it.hasVetoableDelegate 
    }
```

Check if `lazy` properties using `LazyThreadSafetyMode.NONE` or `LazyThreadSafetyMode.PUBLICATION` are `private`:

```kotlin
Konsist
    .scopeFromProject()
    .properties()
    .withLazyDelegate(LazyThreadSafetyMode.NONE, LazyThreadSafetyMode.PUBLICATION)
    .assertTrue { 
        it.hasPrivateModifier 
    }
```

Available built-in delegate checks and list filters:

| Member                         | List filters                                                            | Matches                                        |
| ------------------------------ | ----------------------------------------------------------------------- | ---------------------------------------------- |
| `hasLazyDelegate(mode)`        | `withLazyDelegate(vararg modes)`, `withoutLazyDelegate(vararg modes)`   | `lazy { }`, `lazy<T> { }`, `lazy(mode) { }`    |
| `hasObservableDelegate`        | `withObservableDelegate()`, `withoutObservableDelegate()`               | `Delegates.observable(...)`                    |
| `hasVetoableDelegate`          | `withVetoableDelegate()`, `withoutVetoableDelegate()`                   | `Delegates.vetoable(...)`                      |
| `hasNotNullDelegate`           | `withNotNullDelegate()`, `withoutNotNullDelegate()`                     | `Delegates.notNull()`                          |
| `hasPropertyReferenceDelegate` | `withPropertyReferenceDelegate()`, `withoutPropertyReferenceDelegate()` | `by ::otherProperty`, `by this::otherProperty` |

`withLazyDelegate()` with no modes keeps properties with any `lazy` delegate. With modes, it keeps properties whose `lazy` delegate uses any of the given modes. `withoutLazyDelegate(modes)` removes only those properties, so non-`lazy` properties are kept. Both filters also accept a `Collection<LazyThreadSafetyMode>`.

Custom delegates can be verified using `hasDelegate("delegateName")`.

{% hint style="info" %}
Delegate checks match the delegate text, not resolved types:

* `observable(...)`, `vetoable(...)` and `notNull()` imported directly (without the `Delegates.` prefix) are not detected.
* `by map` delegation is not supported by built-in checks - use `hasDelegate("map")`.
* `hasLazyDelegate(mode)` returns `false` when the mode is not a literal constant, e.g. `lazy(lock) { }` or `lazy(modeVariable) { }`. `hasLazyDelegate()` (no mode) still returns `true`. For the same reason `withLazyDelegate(modes)` drops and `withoutLazyDelegate(modes)` keeps such properties.
* Interface delegation (`class Foo : Bar by baz`) inherits these members. They are designed for property delegates and return `false` for interface delegates.
{% endhint %}

## **Verify Visibility**&#x20;

Property visibility scope can be validated:

Check if property has `internal` modifier:

```kotlin
...
.assertTrue { 
    it.hasInternalModifier
}
```

## **Verify Mutability**&#x20;

Property mutability can be checked.

Check if property is immutable:

```kotlin
...
.assertTrue { 
    it.isVal
}
```

Check if property is mutable:

```kotlin
...
.assertTrue { 
    it.isVar
}
```

Check if public property does not expose a mutable type (e.g. `MutableList`, `ArrayList`, `Array` or a class with a `var` property), including type arguments (e.g. `List<MutableList<String>>`):

```kotlin
Konsist
    .scopeFromProject()
    .properties()
    .withPublicOrDefaultModifier()
    .assertFalse {
        it.type?.isMutableType == true || it.type?.hasMutableTypeArgument == true
    }
```

