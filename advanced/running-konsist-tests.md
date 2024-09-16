# Running Konsist Tests

To execute tests defined in`konsistTest` module run `./gradlew konsistTest:test --rerun-tasks` command.

{% hint style="warning" %}
When running Konsist tests in a multi-module Gradle project, always include the `--rerun-tasks` flag.

The Gradle flag `--rerun-tasks` is essential when Konsist tests are located in a separate module. Without this flag, Gradle assumes the tests are current if the module remains unchanged, leading to test skipping. This becomes problematic with Konsist, as it examines the entire codebase. Consequently, test results may be misleading since Gradle doesn't recognize that these tests are actually evaluating code across multiple modules.
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

{% hint style="info" %}
To execute all unit tests besides tests in the `konsistTest` module run:

`./gradlew test -x konsistTest:test`
{% endhint %}
