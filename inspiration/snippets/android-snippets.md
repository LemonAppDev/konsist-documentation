# Android Snippets

Konsist can be used to guard the consistency of the [Android](https://www.android.com/) project.&#x20;

{% hint style="info" %}
The [android-showcase](https://github.com/igorwojda/android-showcase) project contains set of kotnsist tests.
{% endhint %}
package com.lemonappdev.konsist

import androidx.lifecycle.ViewModel
import com.lemonappdev.konsist.api.Konsist
import com.lemonappdev.konsist.api.ext.list.withParentClassOf
import com.lemonappdev.konsist.api.verify.assert
import com.lemonappdev.konsist.api.verify.assertNot

class AndroidSnippets {
    fun `classes extending 'ViewModel' should have 'ViewModel' suffix`() {
        Konsist
            .scopeFromProject()
            .classes()
            .withParentClassOf(ViewModel::class)
            .assert { it.name.endsWith("ViewModel") }
    }

    fun `no class should use Android util logging`() {
        Konsist
            .scopeFromProject()
            .files
            .assertNot { it.hasImports("android.util.Log") }
    }
}
