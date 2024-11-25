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

Konsist allows you to verify multiple aspects of a properties. For a complete understanding of the available APIs, refer to the language reference documentation for KoPropertyDeclaration[^1].

Let's look at few examples.

## **Verify Name**&#x20;

Property names can be validated to ensure they follow project naming conventions and patterns.

Check if `Boolean` property has name starting with `is`:

```kotlin
...
.assertTrue { 
    it.type?.name == "Boolean" && it.hasNameStartingWith("is")
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

Property delegates can be verified:

Check if property has `lazy` delegate:

```
...
.assertTrue { 
    it.hasDelegate("lazy") 
}
```

## **Verify Visibility**&#x20;

Property visibility scope can be validated:

Check if property has `internal` modifier:

```kotlin
...
.assertTrue { 
    it.isInternal
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

[^1]: 
