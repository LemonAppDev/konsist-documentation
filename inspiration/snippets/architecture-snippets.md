# Architecture Snippets

## 1. 2   L a y e r   A r c h i t e c t u r e   H a s   C o r r e c t   D e p e n d e n c i e s

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

## 2. E v e r y   F i l e   I n   M o d u l e   R e s i d e   I n   M o d u l e   S p e c i f i c   P a c k a g e

```kotlin
@Test
fun `every file in module reside in module specific package`() {
    Konsist
        .scopeFromProject()
        .files
        .assertTrue { it.packagee?.fullyQualifiedName?.startsWith(it.moduleName) }
}
```

## 3. F i l e s   R e s i d e   I n   P a c k a g e   T h a t   I s   D e r i v e d   F r o m   M o d u l e   N a m e

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

