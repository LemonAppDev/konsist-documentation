---
description: Aim for better test separation.
---

# Isolate Konsist Tests

Typically, it's advisable to consolidate all Konsist tests in a unified location. This approach is preferred because these tests are often designed to validate the structure of the entire project's codebase. There are three potential options for storing Konsist tests:

<table><thead><tr><th width="205"></th><th>Android</th><th>Spring</th><th>KMP</th><th>Pure Kotlin</th></tr></thead><tbody><tr><td><a data-mention href="isolate-konsist-tests.md#existing-test-source-set">#existing-test-source-set</a></td><td>✅</td><td>✅</td><td>✅</td><td>✅</td></tr><tr><td><a data-mention href="isolate-konsist-tests.md#dedicated-konsist-test-source-set">#dedicated-konsist-test-source-set</a></td><td>❌</td><td>✅</td><td>✅</td><td>✅</td></tr><tr><td><a data-mention href="isolate-konsist-tests.md#dedicated-module">#dedicated-module</a></td><td>✅</td><td>✅</td><td>✅</td><td>✅</td></tr></tbody></table>

Recommended approach is to use `konsist-test` source set or dedicated module. This approach allows to isolate Konsist tests from other types of tests e.g. separate `unit tests` from `Konsist tests`.

## Existing Test Source Set

The Konsist library can be added to the project by adding the dependency on the existing `test` source set .

![test sorce directory](../.gitbook/assets/TestSourceSet.png)

The downside of this approach is that various types of tests are mixed in `test` source set e.g. `unit tests` and `Konsist tests`.

## Dedicated konsist-test Source Set

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

## Dedicated Module

This section demonstrates how to add the `konsistTest` module to the project. This configuration is primarily helpful for Android projects and Kotlin Multiplatform (KMP) projects, however, this approach will also work with Spring and pure Kotlin projects.

{% hint style="info" %}
The [Android Gradle Plugin](https://developer.android.com/build/releases/gradle-plugin) is used to build Android apps. The Android Gradle Plugin is not compatible with the [JVM Test Suite Plugin](https://docs.gradle.org/current/userguide/jvm\_test\_suite\_plugin.html) and it does not allow adding new source sets. To fully isolate tests a new module is required.

The [Kotlin Multiplatform](https://kotlinlang.org/docs/multiplatform.html) project contains modules with code for different platforms. To decouple Konsist tests from a single platform dedicated module containing Konsist test should be added.
{% endhint %}

### Add `konsistTest` Module:

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

### Add `konsistTest` Module Build Script

{% tabs %}
{% tab title="Gradle Kotlin" %}
Add `konsistTest/build.gradle.kts` file.
{% endtab %}

{% tab title="Gradle Groovy" %}
Add `konsistTest/build.gradle` file.
{% endtab %}
{% endtabs %}

The content of the file will differ depending on the project type (Android, Spring, KMP) and the selected testing framework (JUnit4, JUnit5, KoTest).

Copy content from the file from one of the [starter-projects](https://github.com/LemonAppDev/konsist/tree/main/samples/starter-projects) e.g. copy from `starter-projects/konsist-starter-spring-gradle-groovy-kotest/build.gradle.kts`

Refresh/Sync the Gradle Project in IDE.

### Running Konsist Tests

To execute tests in defined in`konsistTest` module run:

`./gradlew konsistTest:test --rerun-tasks`

{% hint style="warning" %}
When running Konsist tests in a multi-module Gradle project, always include the `--rerun-tasks` flag.

The Gradle flag `--rerun-tasks` is essential when Konsist tests are located in a separate module. Without this flag, Gradle assumes the tests are current if the module remains unchanged, leading to test skipping. This becomes problematic with Konsist, as it examines the entire codebase. Consequently, test results may be misleading since Gradle doesn't recognize that these tests are actually evaluating code across multiple modules.
{% endhint %}

To execute all unit tests besides tests in the `konsistTest` module run:

`./gradlew test -x konsistTest:test`
