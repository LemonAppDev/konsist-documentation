---
description: Boost command line output
---

# Enable Full Command Line Logging

When running using non- [dynamic-konsist-tests.md](dynamic-konsist-tests.md "mention")the default command line output contains only the test name:

```
> Task :konsistTest:test

UseCaseKonsistTest > every use case has single public operator function named 'invoke' FAILED
    com.lemonappdev.konsist.core.exception.KoAssertionFailedException at UseCaseKonsistTest.kt:26

2 tests completed, 1 failed

> Task :konsistTest:testDebugUnitTest FAILED

FAILURE: Build failed with an exception.


```

To be able to see full exception log containing invalid declaration `file path` and `line number` enable `exceptionFormat` in Gradle `testLogging`:

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

Now log output provides all informations relevant to pin point the invalid declaration:

```
> Task :konsistTest:test

UseCaseKonsistTest > every use case has single public operator function named 'invoke' FAILED
    com.lemonappdev.konsist.core.exception.KoAssertionFailedException: Assert 'every use case has single public operator function named 'invoke'' was violated (25 times). Invalid declarations:
    /myproject/usecase/LoginUserUseCase.kt:6:1 (LoginUserUseCase ClassDeclaration)
    /myproject/usecase/GetLocationUseCase.kt:8:1 (GetLocationUseCase ClassDeclaration)
    
2 tests completed, 1 failed

> Task :konsistTest:testDebugUnitTest FAILED

FAILURE: Build failed with an exception.

```



