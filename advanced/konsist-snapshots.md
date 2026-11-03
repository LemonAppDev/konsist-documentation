# Konsist Snapshots

Konsist publishes snapshot versions to the Maven Central [snapshot repository](https://central.sonatype.com/repository/maven-snapshots/com/lemonappdev/konsist/). These snapshots provide early access to new features and bug fixes.

{% hint style="warning" %}
Snapshot versions are development builds and may contain unstable features.
{% endhint %}

## Snapshot Release Process

Snapshots are published automatically:

* Every merge to the `develop` branch publishes the `X.Y.Z-SNAPSHOT` version, where `X.Y.Z` is the next Konsist version (e.g. `1.0.0-beta.1-SNAPSHOT`).
* Every pull request opened in the [Konsist repository](https://github.com/LemonAppDev/konsist) publishes the `X.Y.Z-prN-SNAPSHOT` version, where `N` is the pull request number (e.g. `1.0.0-beta.1-pr2092-SNAPSHOT`).

The list of published versions is available in the [maven-metadata.xml](https://central.sonatype.com/repository/maven-snapshots/com/lemonappdev/konsist/maven-metadata.xml) file.

## How to Use Snapshots

### Add Snapshot Repository

First, you need to include the snapshot repository in your project configuration. Here's how to do it for different build systems:

{% tabs %}
{% tab title="Gradle (Kotlin)" %}
```kotlin
repositories {
    // Konsist snapshot repository
    maven("https://central.sonatype.com/repository/maven-snapshots/")

    // More repositories
}
```
{% endtab %}

{% tab title="Gradle (Groovy)" %}
```groovy
repositories {
    // Konsist snapshot repository
    maven {
        url 'https://central.sonatype.com/repository/maven-snapshots/'
    }
    
    // More repositories
}
```
{% endtab %}

{% tab title="Maven" %}
Add the following repository to the `module/pom.xml` file:

```xml
<repositories>
    <!-- Konsist snapshot repository -->
    <repository>
        <id>konsist-snapshots</id>
        <url>https://central.sonatype.com/repository/maven-snapshots/</url>
        <snapshots>
            <enabled>true</enabled>
        </snapshots>
    </repository>

    <!-- More repositories -->
</repositories>
```
{% endtab %}
{% endtabs %}

### Add Konsist Dependency

To use a Konsist SNAPSHOT dependency, change the version to `X.Y.Z-SNAPSHOT` (versions can be found in [snapshot repository](https://central.sonatype.com/repository/maven-snapshots/com/lemonappdev/konsist/)):

{% tabs %}
{% tab title="Gradle (Kotlin)" %}
Add the following dependency to the `module/build.gradle.kts` file:

```kotlin
dependencies {
    testImplementation("com.lemonappdev:konsist:X.Y.Z-SNAPSHOT")
}
```
{% endtab %}

{% tab title="Gradle (Groovy)" %}
Add the following dependency to the `module/build.gradle` file:

```groovy
dependencies {
    testImplementation "com.lemonappdev:konsist:X.Y.Z-SNAPSHOT"
}
```
{% endtab %}

{% tab title="Maven" %}
Add the following dependency to the `module/pom.xml` file:

```xml
<dependency>
    <groupId>com.lemonappdev</groupId>
    <artifactId>konsist</artifactId>
    <version>X.Y.Z-SNAPSHOT</version>
    <scope>test</scope>
</dependency>
```
{% endtab %}
{% endtabs %}

