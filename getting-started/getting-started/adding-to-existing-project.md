# Adding To Existing Project

Retrofitting Konsist into a project that hasn't been following strict structural guidelines can pose initial challenges, necessitating a thoughtful approach to smoothly transition without disrupting ongoing development.

## Create More Granular Scopes

Scope allows to verification of only a subset of the project code base. When refactoring application scope can be created for a single module to guard specific rules of the improved code base and then further extended to cover already refactored modules. See [koscope.md](../../writing-tests/koscope.md "mention").

## Suppress

It is also possible to suppress checks for files and declarations. See [#suppress](adding-to-existing-project.md#suppress "mention").

