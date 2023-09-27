# When Konsit API Is Not Enough

You may encounter scenarios where Konsist APi is not exposing all required information.

As a workaround, you can process the declaration `text` to retrieve and verify the given declaration.

```kotlin
Konsist
     .scopePromProject()
     .functions()
     .assert { it.text.contains("return") }
```

{% hint style="info" %}
The logic for determining if the function returns a value is more complex (expression body vs. block body, lambdas in side function may have local or non-local returns...), but you should get the idea and write the required checks based on the`text` property.
{% endhint %}

This is just a workaround, so let us know about your case ([sending-us-feedback.md](../help/sending-us-feedback.md "mention")). We will try to add the missing APIs.
