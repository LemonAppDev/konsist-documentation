---
description: Access the Kotlin files using Konsist API
---

# Retrieve The Scope

The [KoScope](https://github.com/LemonAppDev/konsist/blob/main/src/main/kotlin/com/lemon/konsist/core/declaration/KoScope.kt) class is the entry point to the Konsist library. It is the first step in defining the Konsist test. Scope represents a set of Kotlin files to be further queried, filtered ([query-and-filter-declarations.md](query-and-filter-declarations.md "mention")), and verified ([assert.md](assert.md "mention")).

```mermaid
%%{init: {'theme':'forest'}}%%
flowchart TB
    Step1["1. Retrieve The Scope"]-->Step2
    Step2["2. Query and Filter The Declarations"]-->Step3
    Step3["3. Assert"]
    style Step1 fill:#52B523,stroke:#666,stroke-width:2px,color:#fff
```

Every scope contains a set of declarations ([declaration.md](declaration.md "mention")):

```mermaid
%%{init: {'theme':'forest'}}%%
flowchart TD
    KoScope
    KoScope---KoFile
    KoFile---KoClass
    KoFile---KoInterface
    KoFile---KoObject
    KoFile---Other["..."]
    KoClass---KoProperty
    KoClass---KoFunction
```

{% hint style="info" %}
Konsist is built on top of [Kotlin Compiler Psi](https://github.com/JetBrains/kotlin/tree/master/compiler/psi/src/org/jetbrains/kotlin/psi). It wraps the Kotlin compiler parser and provides a simple API to access Kotlin code base declarations. Konsist  [declaration.md](declaration.md "mention") tree mimics the Kotlin code structure:
{% endhint %}

The scope can be created for an entire project, module, package, and a single Kotlin file. The scope is created using Kotlin files present in the project, so the scope will contain more files as the project grows e.g. if the scope represents a single module then every file added to the module will be part of the scope.

## Scope Creation

### Entire Project Scope

The widest scope is the scope containing all Kotlin files present inside the project:

```kotlin
KoScope.fromProjectCodebase() // All Kotlin files present in the project
```

### Module Scope

The `module` argument allows the creation of more granular scopes based on the module name e.g. create a scope containing all Kotlin files present in the `app` module:

```kotlin
KoScope.fromProjectCodebase(module = "app")


project/ 
├─ app/   <--- scope contains all files from the 'app' module
│  ├─ main/
│  │  ├─ App.kt
│  ├─ test/
│  │  ├─ AppTest.kt
├─ core/
│  ├─ main/
│  │  ├─ Core.kt
│  ├─ test/
│  │  ├─ CoreTest.kt
```

### Source Set Scope

The `sourceSet` argument allows the creation of more granular scopes base on the source set name e.g. create a scope containing all Kotlin files present in the `test` source set:

```kotlin
KoScope.fromProjectCodebase(sourceSet = "test")

project/ 
├─ app/
│  ├─ main/
│  │  ├─ App.kt
│  ├─ test/   <--- scope contains all files the 'test' directory
│  │  ├─ AppTest.kt
├─ core/
│  ├─ main/
│  │  ├─ Core.kt
│  ├─ test/   <--- scope contains all files the 'test' directory
│  │  ├─ CoreTest.kt
```

### Production Codebase

The `fromProjectCodebase` method allows the creation of a scope containing only a production code:

```kotlin
KoScope.fromProductionCodebase()

project/ 
├─ app/
│  ├─ main/   <--- scope contains all production code files
│  │  ├─ App.kt
│  ├─ test/
│  │  ├─ AppTest.kt
├─ core/
│  ├─ main/   <--- scope contains all production code files
│  │  ├─ Core.kt
│  ├─ test/
│  │  ├─ CoreTest.kt
```

### Test Codebase

The `fromTestCodebase` method allows the creation of a scope containing only a test code:

```kotlin
KoScope.fromTestCodebase()

project/ 
├─ app/
│  ├─ main/
│  │  ├─ App.kt
│  ├─ test/   <--- scope contains all test code files
│  │  ├─ AppTest.kt
├─ core/
│  ├─ main/
│  │  ├─ Core.kt
│  ├─ test/   <--- scope contains all test code files
│  │  ├─ CoreTest.kt
```

## Package Scope

###



Here is an example of creating scope for all files stored in `usecase` package:

```kotlin
val myScope = KoScope.fromPackageCodebase("..usecase..")
```

{% hint style="info" %}
The double dots (`..`) syntax means zero or more packages. Check the [packageselector.md](packageselector.md "mention") page.
{% endhint %}

Here is an example of creating scope for all files stored in `domain` folder\`:

```kotlin
val myScope = KoScope.fromPathCodebase("/domain")
```

It is also possible to create scope from a single file:

```kotlin
val myScope = KoScope.fromFile("/domain/UseCase.kt")
```

For even more granular control you can use the `KoScope.slice` method to retrieve a scope containing a subset of files from the scope:

```kotlin
// scope containing all files in the 'test' folder
koScope.slice { it.relativePath.contains("/test/") }

// scope containing all files in 'com.domain.usecase' package
koScope.slice { it.hasImport("com.domain.usecase") }

// scope containing all files in 'usecase' package and its sub-packages
koScope.slice { it.hasImport("usecase..") }
```

The `KoScope` can be printed to display a list of all files present in the scope. Here is an example:

```kotlin
println(koScope)
```

## Scope Reuse

Avoid creating scope for every individual test:

<pre class="language-kotlin"><code class="lang-kotlin">// Test.kt
class DataTest {
<strong>    @Test
</strong>    fun `test 1`() {
        KoScope
            .fromProjectCodebase() // Create a new KoScope
            .classes()
            .assert { // .. } 
    }

    fun `test 2`() {
        KoScope
            .fromProjectCodebase() // Create a new KoScope
            .classes()
            .assert { // .. } 
    }
}
</code></pre>

To facilitate testing maintenance scopes should be reused across tests. It is possible by creating a public property and accessing it from multiple tests:

```kotlin
// Scope.kt
val projectScope = KoScope.fromProjectCodebase() // Create a new KoScope

// AppTest.kt
class AppKonsistTest {    
    @Test
    fun `test 1`() {
        projectScope
            .objects()
            .assert { // .. } 
    }
}

// DataTest.kt
class CoreKonsistTest {    
    @Test
    fun `test 1`() {
        projectScope
            .classes()
            .assert { // .. } 
    }

    fun `test 2`() {
        projectScope
            .interfaces()
            .assert { // .. } 
    }
}
```

Here is the file structure representing the above snippet:

```
project/ 
├─ app/
│  ├─ test/
│  │  ├─ app
│  │     ├─ AppKonsistTest.kt
│  │  ├─ core
│  │     ├─ CoreKonsistTest.kt
│  │  ├─ Scope.kt   <--- Instance of the KoScope used in both DataTest and AppTest classes.
```

## Scope Composition

It is possible to compose scopes using Kotlin operators:

```kotlin
// add scopes
val allKoScope = productionScope + testScope

// subtract scopes
val outerLayersScope = allLayersScope - domainLayerScope
```

## Access Specific Declarations

To access specific declaration types such as interfaces, classes, constructors, functions, etc. utilize the [query-and-filter-declarations.md](query-and-filter-declarations.md "mention").
