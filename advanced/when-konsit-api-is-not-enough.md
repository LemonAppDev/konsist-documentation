# When Konsit API Is Not Enough

You may encounter scenarios where Konsist APi is not exposing all of the required information.

As a workaround, you can process the raw declaration `text` to verify the given declaration:

```kotlin
Konsist
     .scopePromProject()
     .functions()
     .assert { it.text.contains("return") }
```

{% hint style="info" %}
The true logic for determining if the function returns a value is more complex.
{% endhint %}

This approach is merely a temporary solution. Let us know about your case ([sending-us-feedback.md](../help/sending-us-feedback.md "mention"))to help us improve Konsist APIs.
