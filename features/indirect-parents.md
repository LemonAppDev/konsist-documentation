# Indirect Parents

The `indirectParents` parameter (`parents()`, `hasParentClasses()`, `hasAllParentInterfacesOf()` methods etc.) specifies whether or not to retrieve parents of the parent (indirect parents). By default, `indirectParents` is `false` e.g.

```mermaid
%%{init: {'theme':'forest'}}%%
flowchart TB
    ClassA-->ClassB-->ClassC
    style ClassC fill:#52B523,stroke:#666,stroke-width:2px,color:#fff
```

For the above inheritance hierarchy it is possible to retrieve:

1. Direct parents of `ClassC` (`ClassB`):

```kotlin
Konsist
	.scopeFromProject()
	.classes()
	.first { it.name == "ClassC" }
	.parents() // ClassB
```

2. All parents present in the codebase hierarchy (`ClassB` and `ClassA`):

```kotlin
Konsist
	.scopeFromProject()
	.classes()
	.first { it.name == "ClassC" }
	.parents(indirectParents = true) // ClassB, ClassA
```

Notice that only parents existing in the project code base are returned.
