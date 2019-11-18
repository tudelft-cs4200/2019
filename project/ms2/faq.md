---
layout: page
title: "Milestone 2 FAQ"
excerpt: "Milestone 2 FAQ"
tags: ["project"]
context: project
subcontext: ms2
---

{% include _toc.html %}

# Miscellaneous

Some students cannot access <http://metaborg.org> because the TU Delft DNS server fails to
resolve. As a workaround you can access the documentation at <http://spoofax.readthedocs.io>.

# Debugging

### How do I know if building my language failed?

Make sure you have the console view open -- use `Window > Show View > Console` to open it. If the
build fails, the build process logs `BUILD FAILED`, otherwise it will show `Reloading language
project eclipse:///minijava`.

Sometimes the build fails with the following exception:

```
  org.metaborg.core.MetaborgException:
    Previous build failed  and no change in the build input has been
    observed, not rebuilding. Fix the problem, or clean and rebuild
    the project to force a rebuild.
```

The build will run again after you clean you project.

### Why do I get exceptions when hovering the file or trying to show analysis results?

Sometimes the Spoofax update seems incomplete, and you get exceptions about missing primitives
(usually named something like `SG_ast_...`). This can be resolved by downloading a fresh Eclipse
with Spoofax ([see instructions](/documentation/spoofax.html#downloading)).

### Why do my tests fail?

There are several possible reasons why your tests do not work.

1. An error `Unable to access the language under test: 'minijava'.` at the top of the file means the
   language was not built, or the build failed ([see here](#build-failure) for more info).
1. An error `Expected analysis to succeed` on a test means analysis has crashed, and the test cannot
   be run. [See here](#build-failure) for info on analysis crashes.
1. Error `Found unexpected matching messages outside the test region` on the test occurs when you
   test for messages (e.g., no errors), but some messages of the expected severity appear in the
   fixture instead of the test. Because this could mean an error in the location of the messages,
   this is disallowed altogether.
1. The test expectation was not met. The error messages give more information on the cause (e.g.,
   there were errors, when none were expected).

Protip: writing small example programs of the case you are investigating can be a quick way to see
if your implementation behaves the way you expect.

### Why does `get-type` fail?

If `get-type` fails when using the `run get-type` test expectation, it prints some debug information
to the console that may help to resolve the problem.

If the console output says `get-type: no analysis for node ...`:

1. The program may have type errors. The `run get-type` expectation does _not_ imply that the program
   is error free. Add a `analysis succeeds` before the`run get-type` to ensure there are no errors.
   
If the console output says `get-type: no type on node ...`:

1. Types may not be correctly associated with the AST nodes (using `@e.type := T`). See the lab
   description on how to do this.
1. The strategy may be run on the wrong part of the AST. Check that the node in the console error is
   the node that should have the type. For example, maybe `get-type` must be applied to a selection
   (with `run get-type on #1 to ...`) instead of on the whole fragment (with `run get-type to ...`).

If the console output says `get-type: no className on scope ...`:

1. The `className` relation may not be defined, or entries are not added (see lab description for details).
1. The `className` entries may be associated with the wrong scope. Ensure you add the `className` to the
   class scope, the one used in the class type.
