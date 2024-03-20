# java.lang.OutOfMemoryError: Java heap space

For large projects with many classes to parse, the default JVM heap size might not suffice. If you encounter `java.lang.OutOfMemoryError: Java heap space` error consider increasing the `maxHeapSize` for the `test` source set:

{% tabs %}
{% tab title="Gradle (Kotlin)" %}
Add the following argument to the`build.gradle.kts` file:

```kotlin
tasks.withType<Test> {
    maxHeapSize = "1g"
}
```
{% endtab %}

{% tab title="Gradle (Groovy)" %}
Add the following argument to the `build.gradle` file:

```groovy
test {
    maxHeapSize = "1g"
}
```
{% endtab %}

{% tab title="Maven" %}
Add the following argument to the `pom.xml` file:

```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-surefire-plugin</artifactId>
    <version>3.0.0</version>
    <configuration>
        <argLine>-Xmx1g</argLine>
    </configuration>
</plugin>
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
You may need to set larger value than 1 gigabyte.
{% endhint %}

