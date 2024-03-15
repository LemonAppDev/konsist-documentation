# Architecture Snippets

Snippets used to guard application architecture.

## 1. 2 Layer Architecture Has Correct Dependencies

```kotlin
@Test
fun `2 layer architecture has correct dependencies`() {
    Konsist
        .scopeFromProject()
        .assertArchitecture {
            val presentation = Layer("Presentation", "com.myapp.presentation..")
            val business = Layer("Business", "com.myapp.business..")
            val persistence = Layer("Persistence", "com.myapp.persistence..")
            val database = Layer("Database", "com.myapp.database..")

            presentation.dependsOn(business)
            business.dependsOn(presentation)
            business.dependsOn(persistence)
            persistence.dependsOn(business)
            business.dependsOn(database)
            database.dependsOn(business)
        }
}
```

## 2. Every File In Module Reside In Module Specific Package

```kotlin
@Test
fun `every file in module reside in module specific package`() {
    Konsist
        .scopeFromProject()
        .files
        .assertTrue { it.packagee?.fullyQualifiedName?.startsWith(it.moduleName) }
}
```

## 3. Files Reside In Package That Is Derived From Module Name

```kotlin
@Test
fun `files reside in package that is derived from module name`() {
    Konsist.scopeFromProduction()
        .files
        .assertTrue {
            /*
            module -> package name:
            feature_meal_planner -> mealplanner
            feature_caloric_calculator -> caloriccalculator
            */
            val featurePackageName = it
                .moduleName
                .removePrefix("feature_")
                .replace("_", "")

            it.hasPackage("com.myapp.$featurePackageName..")
        }
}
```

