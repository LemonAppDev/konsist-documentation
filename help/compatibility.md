---
description: Konsist ecosystem compatibility
---

# Compatibility

Konsist is compatible with all types of Kotlin projects including [Android](https://www.android.com/), [Spring](https://spring.io/), and [Kotlin Multiplatform](https://kotlinlang.org/docs/multiplatform.html) projects.

Konsist works with popular testing frameworks executing Kotlin code. Konsist has first-class support for [JUnit4](https://junit.org/junit4/), [JUnit5](https://junit.org/junit5/), and [Kotest](https://kotest.io/).

Konsist works with Kotest `5.x` and `6.x`. The `KoKotestExtension` (see [kotest-support.md](../features/kotest-support.md "mention")) requires Kotest `6.x`. With Kotest `5.x` pass the `testName` argument instead.

Konsist is compatible with popular build systems such as [Gradle](https://gradle.org/) and [Maven](https://maven.apache.org/).

The `Java 11` is a minimum [Java](https://www.java.com/en) version required to run Konsist.

## Kotlin Compatibility

Konsist parses Kotlin code with its own Kotlin compiler (`kotlin-compiler-embeddable`), so the Kotlin version used by the project does not have to match the Kotlin version used by Konsist. Konsist is compiled with Kotlin API/language version `2.0` and requires `kotlin-stdlib` `2.0.21`+ (Konsist `0.17.x` is backwards compatible with Kotlin `1.8.x`).

| Project Kotlin version | Gradle (JVM, Spring) | Gradle (Android, AGP 8) | Gradle (Android, AGP 9) | Maven  |
| ---------------------- | -------------------- | ----------------------- | ----------------------- | ------ |
| `1.8.x` and older      | ❌                    | ❌                       | -                       | ❌      |
| `1.9.x`                | ✅                    | ✅                       | -                       | ❌ (1)  |
| `2.0.x`                | ✅                    | ✅                       | -                       | ❌ (1)  |
| `2.1.x`                | ✅                    | ✅                       | -                       | ❌ (1)  |
| `2.2.x`                | ✅                    | ✅                       | ✅                       | ❌ (1)  |
| `2.3.x`                | ✅                    | ✅                       | ✅                       | ✅      |
| `2.4.x`                | ✅                    | ✅                       | ✅                       | ✅      |
| **Minimum**            | **`1.9`**            | **`1.9`**               | **`2.2.10`** (2)        | **`2.3`** (1) |

(1) Maven projects using Kotlin older than `2.3` require the `-Xskip-metadata-version-check` compiler argument (see [maven-module-was-compiled-with-an-incompatible-version-of-kotlin.md](known-issues/maven-module-was-compiled-with-an-incompatible-version-of-kotlin.md "mention")).

(2) Minimum Kotlin version supported by the Android Gradle Plugin 9.

{% hint style="info" %}
Kotlin `1.9.x` projects may still fail to compile because of other dependencies requiring newer `kotlin-stdlib` (e.g. `de.mannodermaus.android-junit5` plugin `1.13`+). This is not related to Konsist.
{% endhint %}

## Kotlin Standard Library Compatibility

| `kotlin-stdlib`                 | Gradle                                                    | Maven                                                                     |
| ------------------------------- | --------------------------------------------------------- | ------------------------------------------------------------------------- |
| Version required by Konsist     | `2.0.21`                                                  | `2.0.21`                                                                  |
| Version used by the project     | The highest version (project version or `2.0.21`)          | The version declared by the project (Maven nearest-wins dependency resolution) |
| Minimum version                 | `2.0.21`                                                  | `2.0.21`                                                                  |

Konsist does not upgrade `kotlin-stdlib` used by the project to the version of its own Kotlin compiler.

{% hint style="warning" %}
When a BOM (e.g. Spring Boot dependency management) downgrades `kotlin-compiler-embeddable` to the project Kotlin version, Konsist works, but can't parse Kotlin syntax newer than this version.
{% endhint %}

## Dependencies

Konsist depends on:

* `org.jetbrains.kotlinx:kotlinx-coroutines-core-jvm:1.9.0` (minimal coroutine usage makes Konsist compatible with newer coroutines versions)
* `org.jetbrains.kotlin:kotlin-compiler-embeddable:2.4.21` (without `kotlin-stdlib` dependency)
* `org.jetbrains.kotlin:kotlin-stdlib:2.0.21`

JUnit and Kotest are not Konsist dependencies. `KoJUnitExtension` and `KoKotestExtension` use the test framework already present in the project.
