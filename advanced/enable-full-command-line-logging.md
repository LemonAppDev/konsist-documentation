---
description: >-
  To be able to see full exception log containing failed declaration enable
  `exceptionFormat` in Gradle `testLogging`:
---

# Enable Full Command Line Logging

{% tabs %}
{% tab title="Gradle Kotlin" %}
```kotlin
tasks.withType<Test> {
  testLogging {
    events(TestLogEvent.FAILED)
    exceptionFormat = TestExceptionFormat.FULL
  }
}
```
{% endtab %}

{% tab title="Gradle Groovy" %}
```groovy
tasks.test { 
    testLogging { 
        events(TestLogEvent.FAILED)
        exceptionFormat = TestExceptionFormat.FULL 
    } 
}
```
{% endtab %}
{% endtabs %}
