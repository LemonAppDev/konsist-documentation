# Add Konsist Dependency

### Add Maven Central Repository

Add `mavenCentral` repository:

```
repositories {
    mavenCentral()
}
```

### Add Konsist Dependency

To use Konsist, include the Konsist dependency from Maven Central:

{% tabs %}
{% tab title="Gradle (Kotlin)" %}
Add the following dependency to the `module\build.gradle.kts` file:

```kotlin
dependencies {
    testImplementation("com.lemonappdev:konsist:0.16.1")
}
```
{% endtab %}

{% tab title="Gradle (Groovy)" %}
Add the following dependency to the `module\build.gradle` file:

```groovy
dependencies {
    testImplementation "com.lemonappdev:konsist:0.16.1"
}
```
{% endtab %}

{% tab title="Maven" %}
Add the following dependency to the `module\pom.xml` file:

```xml
<dependency>
    <groupId>com.lemonappdev</groupId>
    <artifactId>konsist</artifactId>
    <version>0.16.1</version>
    <scope>test</scope>
</dependency>
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
See [compatibility.md](../../help/compatibility.md "mention")to learn how Konsist integrates with Kotlin ecosystem.
{% endhint %}

{% hint style="info" %}
To achieve better test separation Konsist can be configured inside a custom`konsistTest` source set or a dedicated `konsistTest` module. See [isolate-konsist-tests.md](../../advanced/isolate-konsist-tests.md "mention") for guidelines on how to store Konsist test in project codebase and how to run them using cmd.
{% endhint %}
