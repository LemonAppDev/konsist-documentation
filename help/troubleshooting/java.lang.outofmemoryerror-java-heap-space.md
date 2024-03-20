# java.lang.OutOfMemoryError: Java heap space

For large projects with many classes to parse, the default JVM heap size might not suffice. If you encounter `java.lang.OutOfMemoryError: Java heap space` errors while running Konsist tests, consider increasing the `maxHeapSize` for the `test` source set:

{% tabs %}
{% tab title="Gradle (Kotlin)" %}
Add the following dependency to the `build.gradle.kts` file:

```kotlin
tasks.withType<Test> {
    maxHeapSize = "1024m"
}
```
{% endtab %}

{% tab title="Gradle (Groovy)" %}
Add the following dependency to the `module\build.gradle` file:

```groovy
test {
    maxHeapSize = "4096m"
}
```
{% endtab %}

{% tab title="Maven" %}
Add the following dependency to the `module\pom.xml` file:

```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-surefire-plugin</artifactId>
    <version>3.0.0</version>
    <configuration>
        <argLine>-Xmx4096m</argLine>
    </configuration>
</plugin>
```
{% endtab %}
{% endtabs %}



