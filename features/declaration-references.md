# Declaration References

Declaration reference represents a link between codebase declarations. Konsist allows to precisely verify properties of linked type. This can be type used in function or property declaration or child/parent class or interface. For example

1\. Verify if all types of function parameters are interfaces:

```kotlin
Konsist
    .scopeFromProject()
    .functions()
    .parameters
    .types
    .assertTrue {
        it.isInterface
    }
```

2\. Access properties of parents (parent classes and child interfaces). Below snippet checks if parent class has `internal` modifier:

```kotlin
fun `all parrent interfaces are internal`() {
    Konsist
        .scopeFromProject()
        .classes()
        .parentInterfaces()
        .assertTrue {
            it.hasInternalModifier()
        }
}
```

3\. Access properties of children (child classes and child interfaces). Below snippet checks if all interfaces have children that resided in `..somepackage..` package:

```kotlin
Konsist
    .scopeFromProject()
    .interfaces()
    .assertTrue {
        it.hasAllChildren(indirectChildren = true) { child -> 
            child.resideInPackage("..somepackage..") 
        }
    }
```

## Type Representation

Kotlin types can defined in multiple ways. Consider `foo` property with `Foo` type:

```
val foo: Foo
```

The `Foo` type can be defined by:

* class
* interface
* object
* type alias
* import alias
* kotlin types (Kotlin basic type or Kotlin collections type)
* function type
* external library (type defined outside project codebase) represents declaration which is not defined in the project

The `Foo`  type can be represented by one of `KoDeclarationX` classes:

<table><thead><tr><th width="464">Sorce</th><th width="282">Declaration</th></tr></thead><tbody><tr><td><a data-mention href="declaration-references.md#type-represented-by-class">#type-represented-by-class</a></td><td><code>KoClassDeclaration</code></td></tr><tr><td><a data-mention href="declaration-references.md#type-represented-by-interface">#type-represented-by-interface</a></td><td><code>KoInterfaceDeclaration</code></td></tr><tr><td><a data-mention href="declaration-references.md#type-represented-by-object">#type-represented-by-object</a></td><td><code>KoObjectDeclaration</code></td></tr><tr><td><a data-mention href="declaration-references.md#type-represented-by-type-apias">#type-represented-by-type-apias</a></td><td><code>KoTypeAliasDeclaration</code></td></tr><tr><td><a data-mention href="declaration-references.md#type-represented-by-import-alias">#type-represented-by-import-alias</a></td><td><code>KoImportAliasDeclaration</code></td></tr><tr><td><a data-mention href="declaration-references.md#type-represented-by-kotlin-type">#type-represented-by-kotlin-type</a></td><td><code>KoKotlinTypeDeclaration</code></td></tr><tr><td><a data-mention href="declaration-references.md#type-represented-function-type">#type-represented-function-type</a></td><td><code>KoFunctionDeclaration</code></td></tr><tr><td><a data-mention href="declaration-references.md#type-represented-by-external-type">#type-represented-by-external-type</a></td><td><code>KoExternalDeclaration</code></td></tr></tbody></table>

Each of these types possesses a largely distinct set of characteristics; for instance, classes and interfaces can include annotations, whereas import aliases cannot.

To access properties the specific declaration type, the deceleration cast to more specific type is required (from generic `KoTypeDeclaration`). Example below assumes that `Foo` is represented by the `Foo` class:

```kotlin
Konsist
    .scopeFromProject()
    .properties()    
    .types
    .assertTrue { koTypeDeclaration ->
        val koClass = koTypeDeclaration as KoClassDeclaration

        koClass.hasAllAnnotations {
            it.representsTypeOf<String>()
        }
    }
```

To facilitate testing Konsist API provides set of dedicated casting extensions. The above code can be simplified:

```kotlin
Konsist
    .scopeFromProject()
    .properties()
    .types
    .assertTrue { koTypeDeclaration ->
        koTypeDeclaration.asSourceClass.hasAllAnnotations {
            it.representsTypeOf<String>()
        }
    }
```

Here is the list of all casting extensions:

<table data-full-width="true"><thead><tr><th>Sorce</th><th width="273">Declaration</th><th>Cast Extension</th><th>Type Check Extension</th></tr></thead><tbody><tr><td><a data-mention href="declaration-references.md#type-represented-by-class">#type-represented-by-class</a></td><td><code>KoClassDeclaration</code></td><td><code>asClassDeclaration</code></td><td><code>isClass</code></td></tr><tr><td><a data-mention href="declaration-references.md#type-represented-by-interface">#type-represented-by-interface</a></td><td><code>KoInterfaceDeclaration</code></td><td><code>asObjectDeclaration</code></td><td><code>isObject</code></td></tr><tr><td><a data-mention href="declaration-references.md#type-represented-by-object">#type-represented-by-object</a></td><td><code>KoObjectDeclaration</code></td><td><code>asInterfaceDeclaration</code></td><td><code>isInterface</code></td></tr><tr><td><a data-mention href="declaration-references.md#type-represented-by-type-apias">#type-represented-by-type-apias</a></td><td><code>KoTypeAliasDeclaration</code></td><td><code>asTypeAliasDeclaration</code></td><td><code>isTypeAlias</code></td></tr><tr><td><a data-mention href="declaration-references.md#type-represented-by-kotlin-type">#type-represented-by-kotlin-type</a></td><td><code>KoImportAliasDeclaration</code></td><td><code>asImportAliasDeclaration</code></td><td><code>isImportAlias</code></td></tr><tr><td><a data-mention href="declaration-references.md#type-represented-function-type">#type-represented-function-type</a></td><td><code>KoKotlinTypeDeclaration</code></td><td><code>asKotlinTypeDeclaration</code></td><td><code>isKotlinType</code></td></tr><tr><td><a data-mention href="declaration-references.md#type-represented-by-external-type">#type-represented-by-external-type</a></td><td><code>KoFunctionDeclaration</code></td><td><code>asFunctionTypeDeclaration</code></td><td><code>isFunctionType</code></td></tr><tr><td><a data-mention href="declaration-references.md#external-types">#external-types</a></td><td><code>KoExternalDeclaration</code></td><td><code>asExternalTypeDeclaration</code></td><td><code>isExternalType</code></td></tr></tbody></table>



### Type Represented By Class

Source code:

```kotlin
internal class Foo
```

Usage:

```kotlin
val foo: Foo? = null
```

Konsist test:

```kotlin
scope
    .properties()
    .types
    .assertTrue {
        it.asClassDeclaration?.hasInternalModifier
    }
```

### Type Represented By Interface

Source code:

```kotlin
internal interface Foo
```

Usage:

```kotlin
val foo: Foo? = null
```

Konsist test:

```kotlin
scope
    .properties()
    .types
    .assertTrue {
        it.asInterfaceDeclaration?.hasInternalModifier
    }
```

### Type Represented By Object

This scenario is uncommon, but still possible.

Source code:

```kotlin
internal object Foo
```

Usage:

```kotlin
val foo: Foo? = null
```

Konsist test:

```kotlin
scope
    .properties()
    .types
    .assertTrue {
        it.asObjectDeclaration?.hasInternalModifier
    }
```

### Type Represented By Type Apias

Source code:

```kotlin
internal object Foo
```

Usage:

```kotlin
typealias MyFoo = Foo
val foo: MyFoo? = null
```

Konsist test:

<pre class="language-kotlin"><code class="lang-kotlin">scope
    .properties()
    .types
    .assertTrue {
<strong>        it
</strong><strong>            .sourceTypeAlias
</strong><strong>            .type
</strong><strong>            .sourceInterface
</strong><strong>            .hasInternalModifier
</strong>    }
</code></pre>

### Type Represented By Import Alias

Source code:

```kotlin
internal object Foo
```

Usage:

```kotlin
import com.app.Foo as MyFoo

val foo: MyFoo? = null
```

Konsist test:

<pre class="language-kotlin"><code class="lang-kotlin">scope
    .properties()
    .types
    .assertTrue {
<strong>        it
</strong><strong>            .asTypeAliasDeclaration
</strong><strong>            .type
</strong><strong>            .asInterfaceDeclaration
</strong><strong>            .hasInternalModifier
</strong>    }
</code></pre>

### Type Represented By Kotlin Type

Source code:

```kotlin
// Kotlin internal source code for String
```

Usage:

```kotlin
val foo: String? = null
```

Konsist test:

<pre class="language-kotlin"><code class="lang-kotlin">scope
    .properties()
    .types
    .assertTrue {
<strong>        it.asKotlinTypeDeclaration.name == "String"
</strong>    }
</code></pre>

### Type Represented Function Type

Source code:

```kotlin
// Kotlin internal source code
```

Usage:

```kotlin
val foo: () -> Unit? = null
```

Konsist test:

<pre class="language-kotlin"><code class="lang-kotlin">scope
    .properties()
    .types
    .assertTrue {
<strong>        it
</strong><strong>            .sourceFunctionType
</strong>            .parameterTypes
            .isEmpty()
    }
</code></pre>

### Type Represented By External Type

External type represents the type is define outside of the project codebase, usually by external library. Konsist is not able to parse this type, so type  information is limited (Konsist is not able to parse the compiled file).

For Example:

```
class MyViewModel: ViewModel
```

The Android `ViewModel` class is provided by `androidx.lifecycle:lifecycle-viewmodel-ktx` dependency, so Konsist has limited information.&#x20;

