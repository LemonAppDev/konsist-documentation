# Maven: Module was compiled with an incompatible version of Kotlin

Maven projects using Kotlin older than `2.3` fail to compile tests:

```
[ERROR] .../kotlin-compiler-embeddable-2.4.21.jar!/META-INF/org.jetbrains.kotlin_backend.jvm.codegen.kotlin_module: (-1, -1) Module was compiled with an incompatible version of Kotlin. The binary version of its metadata is 2.4.0, expected version is 2.1.0.
```

Konsist parses Kotlin files with `kotlin-compiler-embeddable`. Maven adds `runtime` scope dependencies to the test compile classpath, so the project Kotlin compiler reads the metadata of `kotlin-compiler-embeddable`. Kotlin compiler can read metadata only up to one version newer than itself (e.g. Kotlin `2.2` reads metadata up to `2.3`), so Kotlin `2.2` and older can't read `kotlin-compiler-embeddable` `2.4` metadata.

Gradle projects are not affected, because Gradle keeps `runtime` dependencies off the compile classpath.

To fix the issue, use one of these options:

* Upgrade the project Kotlin version to `2.3`+.
* Add the `-Xskip-metadata-version-check` compiler argument to the `kotlin-maven-plugin`:

```xml
<plugin>
    <groupId>org.jetbrains.kotlin</groupId>
    <artifactId>kotlin-maven-plugin</artifactId>
    <version>${kotlin.version}</version>
    <configuration>
        <args>
            <arg>-Xskip-metadata-version-check</arg>
        </args>
    </configuration>
</plugin>
```

{% hint style="warning" %}
The `-Xskip-metadata-version-check` argument disables the metadata version check for all dependencies. Consider moving Konsist tests to a dedicated Maven module, so the argument does not affect other tests (see [isolate-konsist-tests.md](../../advanced/isolate-konsist-tests.md "mention")).
{% endhint %}
