# Running Konsist Tests In Dedicated Gradle Module

Gradle's default behavior assumes that a module's code is up-to-date if the module itself hasn't been modified. This can lead to issues when Konsist tests are placed in a separate module. In such cases, Gradle may skip these tests, believing they're unnecessary.

However, this approach doesn't align well with Konsist's functionality. Konsist analyzes the entire codebase, not just individual modules. As a result, when Gradle skips Konsist tests based on its module-level change detection, it fails to account for potential changes in other modules that Konsist would typically examine.&#x20;

There are few solutions to this problem.

## Solution 1: Module Is Always Out of Date

An alternative solution for this problem is to define `konsistTest` module as always being out of date:

{% tabs %}
{% tab title="Gradle Kotlin" %}
```kotlin
// konsistTest/build.gradle.kts

tasks.withType<Test> {
    outputs.upToDateWhen { false }
}
```
{% endtab %}

{% tab title="Gradle Grovy" %}
```groovy
// konsistTest/build.gradle

tasks.withType(Test) {
    outputs.upToDateWhen { false }
}
```
{% endtab %}
{% endtabs %}

## Solution 2: Flag --rerun-tasks

{% hint style="info" %}
To execute all unit tests besides tests in the `konsistTest` module run:

`./gradlew test -x konsistTest:test`
{% endhint %}

To avoid manually passing `--rerun-tasks` flag each time a custom `konsistCheck` task can be added to the root build config file:

{% tabs %}
{% tab title="Gradle Kotlin" %}
Add to root `build.gradle.kts`:

```kotlin
tasks.register("konsistCheck") {
    group = "verification"
    description = "Runs Konsist static code analysis"

    doLast {
        val output = ByteArrayOutputStream()
        val result = project.exec {
            commandLine("./gradlew", "konsistTest:test", "--rerun-tasks")
            standardOutput = output
            errorOutput = output
            isIgnoreExitValue = true
        }

        println(output.toString())

        if (result.exitValue != 0) {
            throw GradleException("Konsist tests failed")
        }
    }
}
```
{% endtab %}

{% tab title="Gradle Groovy" %}
Add to root `build.gradle`:

```groovy
tasks.register("konsistCheck") {
    group = "verification"
    description = "Runs Konsist static code analysis"

    doLast {
        def output = new ByteArrayOutputStream()
        def result = project . exec {
            commandLine './gradlew', 'konsistTest:test', '--rerun-tasks'
            standardOutput = output
            errorOutput = output
            ignoreExitValue = true
        }

        println output . toString ()

        if (result.exitValue != 0) {
            throw new GradleException ("Konsist tests failed")
        }
    }
}
```
{% endtab %}
{% endtabs %}

After adding `konsistCheck` task run `./gradlew konsistCheck` to execute all Konsist tests.
