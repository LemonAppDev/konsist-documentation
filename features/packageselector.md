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
        .assertTrue { it.resideInPackage("com.app..") }
        
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
        .assertTrue { it.resideInPackage("..logger..") }

// logger - valid  
// com.logger - valid  
// com.logger.tree - valid
// com - invalid
```
