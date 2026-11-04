---
description: Select packages
---

# Package Wildcard

Package wildcard syntax is used to provide a more flexible way of querying packages.

The two dots (`..`) mean zero or more packages, e.g. all classes reside in a package starting with `com.app`:

```kotlin
    Konsist
        .scopeFromProject()
        .classes()
        .assertTrue { it.residesInPackage("com.app..") }
        
// com.app - valid
// com.app.data - valid  
// com.app.data.repository - valid  
// com.data - invalid
// com - invalid
        
```

Package wildcard syntax can be used multiple times inside the string argument. Here all interfaces reside in a package `logger` prefixed and suffixed by any number of packages:

```kotlin
    Konsist
        .scopeFromProject()
        .interfaces()
        .assertTrue { it.residesInPackage("..logger..") }

// logger - valid  
// com.logger - valid  
// com.logger.tree - valid
// com - invalid
```

Package without `..` must match exactly (case-insensitive), e.g. `residesInPackage("com.app")` is `false` for a declaration in the `com.app.data` package. The `..` wildcard can also be used between packages, e.g. `com..data` matches `com.data` and `com.app.data`.

{% hint style="info" %}
For types (`KoTypeDeclaration`) and parents (`KoParentDeclaration`), `packagee` and `residesInPackage` refer to the package of the file where the type is referenced (the use site), not the package where it is declared. To check the package where the type is declared use its source declaration, e.g. `type.sourceDeclaration?.asClassDeclaration()?.residesInPackage("com.app..")`.
{% endhint %}
