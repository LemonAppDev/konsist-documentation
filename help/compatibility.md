---
description: Konsist ecosystem compatibility
---

# Compatibility

Konsist is compatible with all types of Kotlin projects including [Android](https://www.android.com/), [Spring](https://spring.io/), and [Kotlin Multiplatform](https://kotlinlang.org/docs/multiplatform.html) projects.

Konsist works with popular testing frameworks executing Kotlin code. Konsist has first-class support for [JUni4](https://junit.org/junit4/), [JUnit5](https://junit.org/junit5/), and [Kotest](https://kotest.io/).

Konsist is compatible with popular build systems such as [Gradle](https://gradle.org/) and [Maven](https://maven.apache.org/).



The `Java 8` is a minimum [Java](https://www.java.com/en) version required to run Konsist.

Konsist is backwards compatible with Kotlin `1.8.x` ( since Konsist `0.17.0` ).

## Dependencies

Konsist depends on:

* `org.jetbrains.kotlinx:kotlinx-coroutines-core-jvm:1.6.4` (minimal coroutine usage make Konsist compatible with newer coroutines versions)
* `org.jetbrains.kotlin:kotlin-compiler-embeddable:2.0.20`&#x20;
