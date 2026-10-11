---
description: Boost command line output
---

# Enable Full Command Line Logging

By default, Gradle uses the `SHORT` test exception format. It prints only the exception class name and the test line, so the Konsist assertion message (invalid declarations, file paths, `additionalMessage` and reasons added with `addReason`) is not displayed in the command line output:

```
> Task :konsistTest:test

UseCaseKonsistTest > every use case has single public function FAILED
    com.lemonappdev.konsist.core.exception.KoAssertionFailedException at UseCaseKonsistTest.kt:26

1 test completed, 1 failed
```

The message is available only in the HTML test report. To display it in the command line, set `exceptionFormat` to `FULL` in Gradle `testLogging`. Setting `showStackTraces` to `false` hides the stack trace, because it points to Konsist internals, not to the invalid code:

{% tabs %}
{% tab title="Gradle Kotlin" %}
```kotlin
import org.gradle.api.tasks.testing.logging.TestExceptionFormat

tasks.withType<Test>().configureEach {
    testLogging {
        exceptionFormat = TestExceptionFormat.FULL
        showStackTraces = false
    }
}
```
{% endtab %}

{% tab title="Gradle Groovy" %}
```groovy
tasks.withType(Test).configureEach {
    testLogging {
        exceptionFormat = 'full'
        showStackTraces = false
    }
}
```
{% endtab %}
{% endtabs %}

Now the log output contains all information needed to find and fix the invalid declarations:

```
> Task :konsistTest:test

UseCaseKonsistTest > every use case has single public function FAILED
    com.lemonappdev.konsist.core.exception.KoAssertionFailedException: Assert 'every use case has single public function' was violated (2 times). Invalid declarations:
    ├── Class GetUserUseCase file:///project/app/src/main/kotlin/com/myapp/usecase/GetUserUseCase.kt:5:1
    │   └── Has 2 public functions: invoke, validate
    └── Class LoginUserUseCase file:///project/app/src/main/kotlin/com/myapp/usecase/LoginUserUseCase.kt:6:1
        └── Has 2 public functions: invoke, login

1 test completed, 1 failed
```

{% hint style="info" %}
The [starter projects](https://github.com/LemonAppDev/konsist/tree/main/samples/starter-projects) already contain this configuration.
{% endhint %}
