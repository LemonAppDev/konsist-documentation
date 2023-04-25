---
description: Aim for better test separation.
---

# Isolate Konsist Tests

This configuration is optional. The `konsist` library can be added to the project by adding the dependency on the existing `test` source set (see [gettingstarted.md](gettingstarted.md "mention")).

![test sorce directory](../.gitbook/assets/TestSourceSet.png)

As the project grows it may be desirable to isolate tests further e.g. separate `unit tests` from `Konsist tests`. &#x20;

To organize tests a new supplemental test source directory `konsistTest` can be defined.&#x20;

![konsistTest sorce directory](../.gitbook/assets/KonsistTestSourceSet.png)

## Add Test Source Directory

This section demonstrates how to add the `konsistTest` test source directory inside the `app` module.&#x20;

{% hint style="info" %}
This page describes the test located in teh `app` module with the build config file located in `app` a folder. If the project does not contain any modules then configuration should be applied in the root build config file.
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

{% hint style="info" %}
Preconfigured sample projects are in [sample-projects.md](../inspiration/sample-projects.md "mention").
{% endhint %}

The `konsistTest` test source folder works exactly like build-in `test` source folder, so Kosist tests can be defined and executed in a similar way:

{% tabs %}
{% tab title="Gradle" %}
```
 ./gradlew app:konsistTest
```
{% endtab %}

{% tab title="Maven" %}

{% endtab %}
{% endtabs %}

## Konsist As Separate Project

In rare cases where Konsist dependency can't be added to the project, it is possible to create a [koscope.md](../features/koscope.md "mention") containing all Kotlin files from a given path e.g. from another project:

```kotlin
val myScope = KoScope.fromPath("/path/to/project/root")
```



