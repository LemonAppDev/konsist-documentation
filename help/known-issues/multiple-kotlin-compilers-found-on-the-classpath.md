# Multiple Kotlin compilers found on the classpath

Konsist parses Kotlin files with `kotlin-compiler-embeddable`. Other copies of the Kotlin compiler (e.g. `org.jetbrains.kotlin:kotlin-compiler` or Android Lint `intellij-core`) contain classes with the same names. When such a copy is loaded before `kotlin-compiler-embeddable`, parsing fails with `ClassCastException`, `NoClassDefFoundError` or `NoSuchMethodError`. The failure depends on the classpath order, so it may appear only in some environments (e.g. only in CI or only in the IDE).

Konsist detects this and lists every Kotlin compiler found on the classpath:

```
Multiple Kotlin compilers found on the classpath:
 - /.../kotlin-compiler-2.4.21.jar
 - /.../kotlin-compiler-embeddable-2.4.21.jar
Konsist requires only 'kotlin-compiler-embeddable'.
Remove other Kotlin compilers from the classpath (e.g. move Konsist tests to a separate module).
See https://docs.konsist.lemonappdev.com/help/known-issues
```

To fix the issue, remove the other Kotlin compiler from the classpath of Konsist tests. The simplest way is to move Konsist tests to a dedicated source set or Gradle module that does not depend on the other compiler (see [isolate-konsist-tests.md](../../advanced/isolate-konsist-tests.md "mention")).
