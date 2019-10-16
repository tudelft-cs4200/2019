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
