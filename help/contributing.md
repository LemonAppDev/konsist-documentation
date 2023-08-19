---
description: Let's Improve Konsist Together
---

# Contributing

So you want to help? That's great!

The Konsist project is now at a critical stage where community input is essential to polish it.&#x20;

{% hint style="info" %}
Contributing guide is still to be polished, so feel free to [start a new discussion or open an issue](https://github.com/LemonAppDev/konsist/discussions/new/choose) to discuss contributions, features/fixes, and implementation details.&#x20;
{% endhint %}

You may albo contribute and show your support by starring it on [GitHub](https://github.com/LemonAppDev/konsist), tweeting about it, or writing a blog post. This will help to raise awareness of the project and attract more contributors.

## Repositories

Konsist project is spread across multiple repositories:

* [konsist](https://github.com/LemonAppDev/konsist) - repository containing Konist code
* [konsist-documentation](https://github.com/LemonAppDev/konsist-documentation) - repository containing Konsist documentation (this webpage)

## Make a Change In Konsist Repository

This is a high-level view of project contribution:

1. Create a fork of the [konsist](https://github.com/LemonAppDev/konsist) repository
2. Open the fork using [IntelliJ IDEA](https://www.jetbrains.com/idea/)
3. Make changes
4. Add Tests
5. Open the Pull Request.

{% hint style="info" %}
See the [developer readme](https://github.com/LemonAppDev/konsist/blob/main/DeveloperReadme.md) containing more information about the project.
{% endhint %}

## Testing Changes Locally

### Publish Local Konsist Artefact

To test the changes locally you can publish a `SNAPSHOOT` artifact of the Konsist to the local maven repository:

```bash
./gradlew publishToMavenLocal -Pkonsist.releaseTarget=local
```

After publishing a new artifact `x.y.z-SNAPSHOT` with the version number will appear in the local Maven repository:

```
Mac: /Users/<user_name>/.m2/repository/com/lemonappdev/konsist 
Windows: C:\Users\<User_Name>\.m2\repository\com\lemonappdev\konsist 
Linux: /home/<User_Name>/.m2/repository/com/lemonappdev/konsist 
```

To use this artifact you have to add a local Maven repository to your project.

### Use Published Artifact From Local Maven Repository

Every project contains a list of the repositories used to retrieve the dependencies. A local Maven repository has to be manually added to the project.

{% tabs %}
{% tab title="Gradle" %}
Add the following block to the `build.gradle` / `build.gradle.kts` file:

```kotlin
repositories {
    mavenLocal()
}
```
{% endtab %}

{% tab title="Maven" %}
Add the following block to the `module\pom.xml` file:

```xml
<repositories>
    <repository>
        <id>local</id>
        <url>file://${user.home}/.m2/repository</url>
    </repository>
</repositories>
```
{% endtab %}

{% tab title="More" %}
Dependency can be added to other build systems as well. Check the [snippets](https://central.sonatype.com/artifact/com.lemonappdev/konsist) section in the sonatype repository.&#x20;
{% endtab %}
{% endtabs %}

Now build scripts will use the local repository to resolve dependencies, however, the version of Konsist has to be updated to the version of the newly published artifact eg.

`com.lemonappdev:konsist:0.9.0-SNAPSHOT`

Now build scripts will be able to resolve this newly published Konsist artifact.

### Verify Used Konsist Artifact Version

IntelliJ IDEA UI provides a convenient way to check which version of Konsist is used by the project. Open the `External Libraries` section of `Project view` and search for Konsist dependency:

![](<../.gitbook/assets/image (3).png>)

## Make a Change In Konsist Documentation Repository

The [Konsist Documentation repository](https://github.com/LemonAppDev/konsist-documentation) contains this website. Create a fork of the repository, make changes using any text editor (e.g. [Visual Studio Code](https://code.visualstudio.com/)), and open the Pull Request.





