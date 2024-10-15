# Konsist Snapshots

Konsist occasionally releases snapshot versions to a dedicated [snapshot repository](https://s01.oss.sonatype.org/content/repositories/snapshots/com/lemonappdev/konsist/). These snapshots provide early access to new features and bug fixes.

{% hint style="warning" %}
Snapshot versions are development builds and may contain unstable features.
{% endhint %}

## Snapshot Release Process

Currently, snapshots are released manually. At some point this process will be automated - new snapshot will be released each time code is merged to `develop` branch

## How to Use Snapshots

### Add Snapshot Repository

First, you need to include the snapshot repository in your project configuration. Here's how to do it for different build systems:

{% tabs %}
{% tab title="Gradle (Kotlin)" %}
```kotlin
repositories {
    // Konsist snapshot repository
    maven("https://s01.oss.sonatype.org/content/repositories/snapshots/")

    // More repositorues
}
```
{% endtab %}

{% tab title="Gradle (Groovy)" %}
```groovy
repositories {
    // Konsist snapshot repository
    maven {
        url 'https://s01.oss.sonatype.org/content/repositories/snapshots/'
    }
    
    // More repositories
}
```
{% endtab %}

{% tab title="Maven" %}
Add the following dependency to the `module\pom.xml` file:

```xml
<repositories>
    <!-- Konsist snapshot repository -->
    <repository>
        <id>konsist-snapshots</id>
        <url>https://s01.oss.sonatype.org/content/repositories/snapshots/</url>
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

To use Konsist SNAPSHOT dependency changing version to `X.Y.Z-SNAPSHOT` (versions can be found in [snapshot repository](https://s01.oss.sonatype.org/content/repositories/snapshots/com/lemonappdev/konsist/)):

{% tabs %}
{% tab title="Gradle (Kotlin)" %}
Add the following dependency to the `module\build.gradle.kts` file:

```kotlin
dependencies {
    testImplementation("com.lemonappdev:konsist:X.Y.Z-SNAPSHOT")
}
```
{% endtab %}

{% tab title="Gradle (Groovy)" %}
Add the following dependency to the `module\build.gradle` file:

```groovy
dependencies {
    testImplementation "com.lemonappdev:konsist:X.Y.Z-SNAPSHOT"
}
```
{% endtab %}

{% tab title="Maven" %}
Add the following dependency to the `module\pom.xml` file:

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

