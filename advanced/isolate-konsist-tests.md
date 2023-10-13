---
description: Aim for better test separation.
---

# Isolate Konsist Tests

The `konsist` library can be added to the project by adding the dependency on the existing `test` source set (see [Broken link](broken-reference "mention")).

![test sorce directory](../.gitbook/assets/TestSourceSet.png)

As the project grows it may be desirable to isolate tests further e.g. separate `unit tests` from `Konsist tests`. &#x20;

To organize tests add a new test directory, module, or project. See preconfigured [starter-projects.md](../inspiration/starter-projects.md "mention").

## Dedicated Source Set (Spring and Kotlin Projects)

This section demonstrates how to add the `konsistTest` test source directory inside the `app` module. This configuration is mostly useful for Spring and Kotlin projects.

{% hint style="info" %}
This page describes the test located in the `app` module with the build config file located in `app` a folder. If the project does not contain any module then configuration should be applied in the root build config file.
{% endhint %}

This test directory will have a `kotlin` folder containing Kotlin code.

{% tabs %}
{% tab title="Gradle (Kotlin)" %}
Use the Gradle built-in [JVM Test Suite Plugin](https://docs.gradle.org/current/userguide/jvm\_test\_suite\_plugin.html) to define the `konsistTest` source set. Add a `testing` block to the project configuration:

```kotlin
// app/build.gradle.kts

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
                implementation("com.lemonappdev:konsist:0.7.4") 
            }
        }
    }
}

// Optional block to run Konsist tests together with the Gradle 'check' task
tasks.named("check") { 
    dependsOn(testing.suites.named("konsistTest"))
}
```
{% endtab %}

{% tab title="Gradle (Groovy)" %}
Use the Gradle built-in [JVM Test Suite Plugin](https://docs.gradle.org/current/userguide/jvm\_test\_suite\_plugin.html) to define the `konsistTest` source set. Add a `testing` block to the project configuration:

```kotlin
// app/build.gradle

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
                implementation "com.lemonappdev:konsist:0.7.4"
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

// Optional block to run Konsist tests together with the Gradle 'check' task
tasks.named('check') { 
    dependsOn(testing.suites.konsistTest)
}
```
{% endtab %}

{% tab title="Maven" %}
Use the [Maven Build Helper Plugin](https://www.mojohaus.org/build-helper-maven-plugin/) to define the `konsistTest` test source directory. Add plugin config to the project configuration:

````xml
// app/pom.xml

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
````
{% endtab %}
{% endtabs %}

&#x20;Create `app/src/konsistTest/kotlin` folder and reload the project. The IDE will present a new `konsistTest` source set in the `app` module.

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

## Dedicated Module (Android Projects)

This section demonstrates how to add the `konsistTest` module to the project. This configuration is primarily helpful for Android projects, however, it will work with Spring and Kotlin projects.

{% hint style="info" %}
The [Android Gradle Plugin](https://developer.android.com/build/releases/gradle-plugin) is used to build Android apps. The Android Gradle Plugin is not compatible with the [JVM Test Suite Plugin](https://docs.gradle.org/current/userguide/jvm\_test\_suite\_plugin.html) and it does not allow adding new source sets. To fully isolate tests a new module is required.
{% endhint %}

{% tabs %}
{% tab title="Gradle (Kotlin)" %}
Use the Gradle built-in [JVM Test Suite Plugin](https://docs.gradle.org/current/userguide/jvm\_test\_suite\_plugin.html) to define the `konsistTest` source set. Add a `testing` block to the project configuration:

Add `':konsist_test'` to `include` item inside `settings.gradle` file:

```kotlin
// settings.gradle.kts
include("app", ":konsist_test").kts
```

Create `konsist_test/scr/test/kotlin` folder in the project root:

<img src="../.gitbook/assets/image (4) (1).png" alt="" data-size="original">

For Android projects add `com.android.library` plugin in the `konsist_test/scr/test/kotlin/build.gradle.kts` file.

Refresh/Sync the Gradle Project in IDE.
{% endtab %}

{% tab title="Gradle (Groovy)" %}
Use the Gradle built-in [JVM Test Suite Plugin](https://docs.gradle.org/current/userguide/jvm\_test\_suite\_plugin.html) to define the `konsistTest` source set. Add a `testing` block to the project configuration:

Add `':konsist_test'` to `include` item inside `settings.gradle` file:

```kotlin
// settings.gradle
include ':app', ':konsist_test'
```

Create `konsist_test/scr/test/kotlin` folder in the project root:

<img src="../.gitbook/assets/image (4) (1).png" alt="" data-size="original">

For Android projects add `com.android.library` plugin in the `konsist_test/scr/test/kotlin/build.gradle` file.

Refresh/Sync the Gradle Project in IDE.
{% endtab %}
{% endtabs %}

Kosist tests will be defined in the `konsistTest` module. To execute Konsist tests run:

{% tabs %}
{% tab title="Gradle" %}
```
./gradlew konsistTest:test
```
{% endtab %}
{% endtabs %}

To execute all unit tests besides tests in the `konsistTest` module run:

{% tabs %}
{% tab title="Gradle" %}
`./gradlew test -x konsistTest:test --rerun-tasks`
{% endtab %}
{% endtabs %}

{% hint style="danger" %}
The `--rerun-tasks` Gradle flag is required - when Konsist tests are placed in a distinct module and there haven't been changes within that module, Gradle assumes the "unit tests" are up-to-date and might skip them. This can lead to misleading test outcomes, as Gradle isn't aware that these tests are actually evaluating other modules.
{% endhint %}

## Dedicated&#x20;

In rare cases where Konsist dependency can't be added to the project, it is possible to create a [koscope.md](../writing-tests/koscope.md "mention") containing all Kotlin files from a given path e.g. from another project:

```kotlin
val myScope = KoScope.fromPath("/path/to/project/root")
```



