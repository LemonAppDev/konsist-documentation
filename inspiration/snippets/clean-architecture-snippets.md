# Clean Architecture Snippets

## Snippet 1

```kotlin
fun `clean architecture layers have correct dependencies`() {
    Konsist
        .scopeFromProject()
        .assertArchitecture {
            // Define layers
            val domain = Layer("Domain", "com.myapp.domain..")
            val presentation = Layer("Presentation", "com.myapp.presentation..")
            val data = Layer("Data", "com.myapp.data..")

            // Define architecture assertions
            domain.dependsOnNothing()
            presentation.dependsOn(domain)
            data.dependsOn(domain)
        }
}
```

## Snippet 2

```kotlin
fun `classes with 'UseCase' suffix should reside in 'domain' and 'usecase' packages`() {
    Konsist.scopeFromProject()
        .classes()
        .withNameEndingWith("UseCase")
        .assert { it.resideInPackage("..domain..usecase..") }
}
```

## Snippet 3

```kotlin
fun `classes with 'UseCase' suffix should have single method named 'invoke'`() {
    Konsist.scopeFromProject()
        .classes()
        .withNameEndingWith("UseCase")
        .assert { it.declarations().toList().size == 1 && it.containsFunction("invoke") && it.isPublicOrDefault() }
}
```

## Snippet 4

```kotlin
fun `interfaces with 'Repository' annotation should reside in 'data' package`() {
        Konsist.scopeFromProject()
            .interfaces()
            .withAnnotationOf<Repository>()
            .assert { it.resideInPackage("..data..") }
}
```
