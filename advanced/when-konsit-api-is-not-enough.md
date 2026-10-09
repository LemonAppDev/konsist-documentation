# When Konsist API Is Not Enough

You may encounter scenarios where Konsist API is not exposing all of the required information.

As a workaround, you can process the raw declaration `text` to verify the given declaration:

```kotlin
Konsist
    .scopeFromProject()
    .functions()
    .assertTrue { it.text.contains("return") }
```

{% hint style="info" %}
The true logic for determining if the function returns a value is more complex.
{% endhint %}

This approach is merely a temporary solution. Let us know about your case ([getting-help.md](../help/getting-help.md "mention")) to help us improve Konsist APIs.
