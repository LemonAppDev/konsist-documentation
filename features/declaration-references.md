# Declaration References

Declaration references provides link between declarations. Konsist now works with declarations directly, allowing you to precisely verify type properties, inheritance relationships, and more.

For example it is possible to verify if all types of function parameters are interfaces...

```kotlin
fun `all function parameters are interfaces`() {
    Konsist
        .scopeFromProject()
        .functions()
        .parameters
        .types
        .assertTrue {
            it.isInterface
        }
}


```

..or access properties of parents, for example check if parent class has `internal` modifier..

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

.. or filter and verify given type declarations:&#x20;

```kotlin
Konsist
	.scopeFromProject()
	.functions()
	.returnTypes
	.sourceFunctionTypes
	.assertTrue { it.text == "(Int) -> Unit" }
```

## Types

Types in Kotlin can be defined in many ways, so Konsist API contains multiple representations:

* `KoClassDeclaration` represents class
* `KoInterfaceDeclaration` represents interface
* `KoObjectDeclaration` represents object
* `KoTypeAliasDeclaration` represents type alias
* `KoImportAliasDeclaration` represents import alias
* `KoKotlinTypeDeclaration` represents kotlin basic types and collections
* `KoFunctionDeclaration` represents function type
* `KoExternalDeclaration` represents declaration which is not defined in the project



`sourceDeclaration`

