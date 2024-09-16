---
description: Aim for better test separation.
---

# Isolate Konsist Tests

Typically, it's advisable to consolidate all Konsist tests in a unified location. This approach is preferred because these tests are often designed to validate the structure of the entire project's codebase. There are three potential options for storing Konsist tests in project codebase:

<table><thead><tr><th width="205"></th><th>Android</th><th>Spring</th><th>KMP</th><th>Pure Kotlin</th></tr></thead><tbody><tr><td><a data-mention href="isolate-konsist-tests.md#existing-test-source-set">#existing-test-source-set</a></td><td>✅</td><td>✅</td><td>✅</td><td>✅</td></tr><tr><td><a data-mention href="isolate-konsist-tests.md#dedicated-konsist-test-source-set">#dedicated-konsist-test-source-set</a></td><td>❌</td><td>✅</td><td>✅</td><td>✅</td></tr><tr><td><a data-mention href="isolate-konsist-tests.md#dedicated-module">#dedicated-module</a></td><td>✅</td><td>✅</td><td>✅</td><td>✅</td></tr></tbody></table>

Recommended approach is to use [#dedicated-konsisttest-source-set](isolate-konsist-tests.md#dedicated-konsisttest-source-set "mention") or a [#dedicated-module](isolate-konsist-tests.md#dedicated-module "mention"). These approaches allows to easily isolate Konsist tests from other types of tests e.g. separate `unit tests` from `Konsist tests`.

## Existing Test Source Set

The Konsist library can be added to the project by adding the dependency on the existing `test` source set .

![test sorce directory](../.gitbook/assets/TestSourceSet.png)

To execute tests run `./gradlew test` command.

The downside of this approach is that various types of tests are mixed in `test` source set e.g. `unit tests` and `Konsist tests`.

## Dedicated konsistTest Source Set

This section demonstrates how to add the `konsistTest` test source directory inside the `app` module. This configuration is mostly useful for Spring and Kotlin projects.

{% hint style="info" %}
This page describes the test located in the `app` module with the build config file located in `app` a folder. If the project does not contain any module then configuration should be applied in the root build config file.
{% endhint %}

This test directory will have a `kotlin` folder containing Kotlin code.

{% tabs %}
{% tab title="Gradle (Kotlin)" %}
Use the Gradle built-in [JVM Test Suite Plugin](https://docs.gradle.org/current/userguide/jvm\_test\_suite\_plugin.html) to define the `konsistTest` source set. Add a `testing` block to the project configuration:

```kotlin
// build.gradle.kts (root)

plugins {
    `jvm-test-suite`
}

testing {
    suites {
        register("konsistTest", JvmTestSuite::class) {
            dependencies {
                // Add 'main' source set dependency
                implementation(project())
                
                // Add Konsist dependency
                implementation("com.lemonappdev:konsist:0.13.0") 
            }
        }
    }
}

// Optional : Remove Konsist tests from the 'check' task if it exists
tasks.matching { it.name == "check" }.configureEach {
  setDependsOn(dependsOn.filter { it.toString() != "konsistTest" })
}
```
{% endtab %}

{% tab title="Gradle (Groovy)" %}
Use the Gradle built-in [JVM Test Suite Plugin](https://docs.gradle.org/current/userguide/jvm\_test\_suite\_plugin.html) to define the `konsistTest` source set. Add a `testing` block to the project configuration:

```kotlin
// build.gradle (root)

plugins {
    id 'jvm-test-suite'
}

testing {
    suites { 
        test { 
            useJUnitJupiter() 
        }

        konsistTest(JvmTestSuite) { 
            dependencies {
                // Add 'main' source set dependency
                implementation project() 
                
                // Add Konsist dependency
                implementation "com.lemonappdev:konsist:0.13.0"
            }

            targets { 
                all {
                    testTask.configure {
                        shouldRunAfter(test)
                    }
                }
            }
        }
    }
}

// Optional: Remove Konsist tests from the 'check' task if it exists
tasks.matching { it.name == "check" }.configureEach { task ->
    task.setDependsOn(task.getDependsOn().findAll { it.toString() != "konsistTest" })
}
```
{% endtab %}

{% tab title="Maven" %}
Use the [Maven Build Helper Plugin](https://www.mojohaus.org/build-helper-maven-plugin/) to define the `konsistTest` test source directory. Add plugin config to the project configuration:

```xml
# app/pom.xml

<plugin>
    <groupId>org.codehaus.mojo</groupId>
    <artifactId>build-helper-maven-plugin</artifactId>
    <version>3.3.0</version>
    <executions>
        <execution>
            <id>add-konsist-test-source</id>
            <phase>generate-test-sources</phase>
            <goals>
                <goal>add-test-source</goal>
            </goals>
            <configuration>
                <sources>
                    <source>${project.basedir}/src/konsistTest/kotlin</source>
                </sources>
            </configuration>
        </execution>
    </executions>
</plugin>
```
{% endtab %}
{% endtabs %}

Create `app/src/konsistTest/kotlin` folder and reload the project. The IDE will present a new `konsistTest` source set in the `app` module.

<figure><img src="../.gitbook/assets/KonsistTestSourceSet.png" alt=""><figcaption><p>konsistTest sorce directory</p></figcaption></figure>

The `konsistTest` test source folder works exactly like the build-in `test` source folder, so Kosist tests can be defined and executed in a similar way:

{% tabs %}
{% tab title="Gradle" %}
```
./gradlew app:konsistTest
```
{% endtab %}

{% tab title="Maven" %}
```yaml
mvn test
```
{% endtab %}
{% endtabs %}

## Dedicated Gradle Module

This section demonstrates how to add the `konsistTest` module to the project. This configuration is primarily helpful for Android projects and Kotlin Multiplatform (KMP) projects, however, this approach will also work with Spring and pure Kotlin projects.

{% hint style="info" %}
The [Android Gradle Plugin](https://developer.android.com/build/releases/gradle-plugin) is used to build Android apps. The Android Gradle Plugin is not compatible with the [JVM Test Suite Plugin](https://docs.gradle.org/current/userguide/jvm\_test\_suite\_plugin.html) and it does not allow adding new source sets. To fully isolate tests a new module is required.

The [Kotlin Multiplatform](https://kotlinlang.org/docs/multiplatform.html) project contains modules with code for different platforms. To decouple Konsist tests from a single platform dedicated module containing Konsist test should be added.
{% endhint %}

### Add Gradle `konsistTest` Module:

{% tabs %}
{% tab title="Gradle (Kotlin)" %}
Create `konsistTest/src/test/kotlin` directory in the project root:

<figure><img src="../.gitbook/assets/image (31).png" alt="" width="374"><figcaption></figcaption></figure>

Add module include inside `settings.gradle.kts` file:

```kotlin
// settings.gradle.kts
include(":konsistTest")
```
{% endtab %}

{% tab title="Gradle (Groovy)" %}
Create `konsistTest/src/test/kotlin` directory in the project root:

<figure><img src="../.gitbook/assets/image (30).png" alt="" width="374"><figcaption></figcaption></figure>

Add module include inside `settings.gradle.kts` file:

```kotlin
// settings.gradle
include ':konsistTest'
```

For Android projects add `com.android.library` plugin in the `konsistTest/scr/test/kotlin/build.gradle` file.

Refresh/Sync the Gradle Project in IDE.
{% endtab %}
{% endtabs %}

### Running Konsist Tests Stored In A Dedicated Gradle Module

Gradle's default behavior assumes that a module's code is up-to-date if the module itself hasn't been modified. This can lead to issues when Konsist tests are placed in a separate module. In such cases, Gradle may skip these tests, believing they're unnecessary.

However, this approach doesn't align well with Konsist's functionality. Konsist analyzes the entire codebase, not just individual modules. As a result, when Gradle skips Konsist tests based on its module-level change detection, it fails to account for potential changes in other modules that Konsist would typically examine.&#x20;

There are few solutions to this problem.

#### Solution 1: Module Is Always Out of Date

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

#### Solution 2: Flag --rerun-tasks

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
