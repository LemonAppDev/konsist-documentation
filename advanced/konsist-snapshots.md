# Konsist Snapshots

Occasionally  Konsist snapshots are released to snapshots repo.

{% hint style="info" %}
At some point this process will be automated - new snapshot will be released each time code is merged to `develop` branch
{% endhint %}

To use snapshot add snapshot repository and update Konsist version.

### Add Snapshot Repository

To use Konsist snapshot, include the snapshot repository in your project:

{% tabs %}
{% tab title="Gradle (Kotlin)" %}
```kotlin
repositories {
    // Konsist snapshot repository
    maven {
        url = uri("https://s01.oss.sonatype.org/content/repositories/snapshots/")
    }

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

Include the Konsist snapshot dependency:

{% tabs %}
{% tab title="Gradle (Kotlin)" %}
Add the following dependency to the `module\build.gradle.kts` file:

```kotlin
dependencies {
    testImplementation("com.lemonappdev:X.Y.Z-SNAPSHOT")
}
```
{% endtab %}
{% endtabs %}



### Add Konsist Dependency

To use Konsist, include the Konsist dependency from Maven Central:

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

{% hint style="info" %}
To a
{% endhint %}

