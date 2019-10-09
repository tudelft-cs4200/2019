---
layout: page
title: "Lab 5: Name Analysis"
excerpt: "Lab 5: Name Analysis"
tags: ["project"]
context: project
subcontext: ms2
---

{% include _toc.html %}

This lab is under construction. Proceed at own risk.
{: .notice .notice-warning}

In this lab, you define name bindings and corresponding constraints for MiniJava in Statix. The
concepts you are going to use in Statix are described in the following papers:

1. H. van Antwerpen, C. B. Poulsen, A. Rouvoet, E. Visser: [Scopes as Types](https://researchr.org/publication/AntwerpenPRV18), OOPSLA 2018
2. P. Neron, A. Tolmach, E. Visser, G. Wachsmuth: [A Theory of Name Resolution](https://researchr.org/publication/TUD-SERG-2015-001), ESOP 2015
3. H. van Antwerpen, P. Neron, A. Tolmach, E. Visser, G. Wachsmuth: [A Constraint Language for Static Semantic Analysis based on Scope Graphs](https://researchr.org/publication/AntwerpenNTVW16), PEPM 2016

## Overview

### Objectives

Specify name analysis for MiniJava in
Statix by specifying a scope graph and resolution constraints. The specification should include:

1. Scope graph constraints for
  * class declarations
  * class references
  * field declarations
  * parameter declarations
  * variable declarations
  * field, parameter, and variable references
2. Resolution constraints for
  * class references
  * parameter references
  * field references
  * variable references
3. Constraints for
  * duplicate definitions of classes, fields, parameters, and variables
  * field, parameter, and variable declarations that hide parameter or field declarations

### Submission

You need to submit your MiniJava project with a merge request against branch `assignment-5-submission` on GitLab.
The [Git documentation](/documentation/git.html#submitting-an-assignment) explains how to file such a request.

The deadline for submissions is October 26, 2019, 23:59.
{: .notice .notice-warning}

### Grading

You can earn up to 100 points for the correctness of your name analysis.
Therefore, we run several test cases against your implementation.
You earn points, when your implementation passes test cases.
The total number of points depends on how many test cases you pass in each of the following groups:

* name binding (45 points)
  * class declarations and references (6 points)
  * field declarations (6 points)
  * parameter declarations (6 points)
  * variable declarations and references (6 points)
  * scopes of program, classes, and methods (12 points)
  * variable origins (3 points)
* constraints (45 points)
  * unresolved references (16 points)
  * duplicate definitions (20 points)
  * hiding variables and fields (9 points)
* challenge (10 points)

### Early Feedback

We provide early feedback for your language implementation.
This feedback gives you an indication which parts of your implementation might still be wrong.
It includes a summary on how many tests you pass and how many points you earn by passing them.
You have 3 early feedback attempts.

## Detailed Instructions

### Preliminaries

#### GitLab Repository

You continue with your work from the previous assignment. See the
[Git documentation](/documentation/git.html#continue-from-previous-assignment) on
how to create the `assignment-5-develop` branch from your previous work.

### Typing rules

**You have to write rules for all language constructs**

Name binding is specified through name binding and typing rules in a `.stx` file. Statix files must go in
the `trans/analysis` directory. The module name at the top of the file should match the filename relative to `trans`. For example, the file `trans/analysis/minijava.stx` starts as:

```
module analysis/minijava
```

Type checking is specified using predicates, whose rules determine which programs are well-formed and which are not.
The rules typically match on the syntactic constructs of the language.
The rule body specifies the premises that must hold for the construct to be well-formed.
Each predicate is has a signature that describes the types of the input arguments.
Predicates and rules are defined in the `rules` section of the specification.
For example, the signature and two of its rules of the predicate to check expressions, looks like this:

```
rules

   expOk : scope * Exp                           // signature for predicate with two parameters
   
   expOk(s, Call(e, x, args)) :- {y z} PREMISES. // rule for the case Call (end in a dot!)

   expOk(s, True()).                             // rule for the case True
```

The signature specifies that the predicate `expOk` takes two parameters.
The first argument must be of the built-in type `scope`, the second of the type `Exp`, which comes from the signature of the language.
This example shows two forms for predicate rules.
The first (for `Call`) is the full form, matches on the `Call` construct in the head of the rule (the part before `:-`), defines local variable `y` and `z`, followed by a body containing comma-separated premises.
The second is an abbreviated form for rules that have no premises.
Any variables that appear in the head are bound in the body of the rule.
Furthermore, extra local variables are declared between curly braces at the beginning of the body.

Rules of one predicate must not overlap, that is, they must not match on the same constructs as other rules.
Overlapping rules are statically detected, and Statix gives an error on those.
{: .notice .notice-info}

There are several premises (that we also call constraints), that can be part of the rule body.
The simplest are `true` (which always succeeds), `false`, which never succeeds.
For example, the abbreviated rule is equal to `expOk(s, True()) :- true.`

It is also possible to use predicates as premises.
For example, the `Call` rule may enforce well-formedness of the nested expression by having a premise `expOk(s, e)`.

The initial project contains a `programOk` rule that matches the root AST node of a Mini Java program.
Type checking proceeds from this predicate, by matching rules against arguments, and unfolding to the premises of matching rules.
If all premises can eventually be resolved, the program is considered well-formed.

For this lab you have to write predicate rules for all the constructs in the language. Check the signature you have to use in the Statix file in the template project.
Note that the grammar injections are explicit in this signature, which means you have to wrap some constructors in the injection constructor.

### Terms and Sorts

**AST, names, types are all terms, Statix is fully typed**

... term and pattern syntax ...

... sorts ...

### Name Binding

Modeling name binding involves the following:
- Creating scopes and connecting them with edges
- Adding type, method, field, and variable declarations in the scope graph
- Resolving type, field, and variable references and connecting resolution results to the AST
- More sophisticated checks like overriding, duplicate names, hiding

#### Scopes and Edges

**Construct the scope graph corresponding to the rules**

Scope graph is constructed from scopes and edges using the predicates:

* `new s`
* `s -l-> s`

The labels must be defined in the signature,

```
signature
  name-resolution
    labels // ... space separated list of labels ...
```

#### Declarations in the Scope Graph

**Add declarations for the different kinds of names**

Declarations are written as occurrences, which combine a name with a namespace and its AST position.

Namespaces are declared in the signature, and specify the type of the name.
In this lab, the names should always be `ID`s, but in general they can be arbitrary terms.

```
signature
  namespaces
    Var : ID
```

Declarations are added to the scope graph using `s -> OCCURRENCE`.
This adds it to the implicit relation `decl : occurrence`.
Relations may be defined of extra information has to be associated with the occurrence.
For example, one could define a relation that associates a scope with an occurrence:

```
signature
  relations
    scopeOf : occurrence -> scope
```

In this case, one adds data like `s -> OCCURRENCE with scopeOf SCOPE`.

#### Resolving Names

**Resolve references**

Names are resolved using `OCCURRENCE in s |-> ps` in scope `s` to paths `ps`.
In this form, it resolves in the `decl` relation.
The result `ps` contains a list of pairs of `path`s and the data from the relation.
To match on a single variable, e.g., you could write `Var{x@-} in s |-> [(_, Var{x'@_})]`, where `x'` is bound to the name of the original declaration.

To resolve in another relation, e.g. `scopeOf OCCURRENCE in s |-> _`.

The well-formedness and disambiguation of resolution are controlled in the signature per namespace.

```
signature
  name-resolution
    resolve Ns filter REGEX min LABELORD
```

#### Editor Resolution

**Connecting resolution to the AST**

To enable name resolution in the editor, we have to link references to their declarations.
This is done with the `ref` property on AST nodes, as `x.ref := x'`, where `x` is the reference name, and `x'` the name matched from the declaration in the query result.

#### Duplicate definitions

**Checking duplicate definitions**

#### Name hiding

MiniJava requires errors in a few cases where hiding occurs. Fields are not allowed to
hide fields in super classes. Parameters are not allowed to hide fields and local variables are not allowed
to hide fields. MiniJava does also not allow a parameter to have the
same name as a local variable in a method.

TODO: Making warnings

### Overriding

**Checking overriding**

In lab 7 we need to test that an overriding method has the same type as the overridden method in the
parent class. For every method declaration, we want to introduce a reference, that resolves to its
overridden method. For example, consider the following situation:

```
class Foo {
    public int m() {
        return 1;
    }
}
class Bar extends Foo {
    public int m() {
        return 1;
    }
}
```

Queries allow us to ask more powerful questions.
The resolution premises from the previous section are all explicated to full queries.
To see the full query form, use `Syntax > Format Normalized` on the specification.
The full query format is

```
query REL filter REGEX and MATCH min LABELORD and SHADOW in SCOPE |-> RESULT
```

Relation can be a relation name, `decl`, or `()` to only look at the reachable scopes.

Regex specifies well-formed paths ...
The well-formedness is a regular expression on path labels, that rules out paths that do not match
it. In the default case, it means that after an import, you cannot resolve in a parent scope
anymore. The regular expressions on labels have the following syntax:

* `e` is the empty string
* `0` is the empty language (i.e., it matches nothing)
* `<Label>` is a literal label
* `~<Regexp>` is the complement (e.g., `~0` matches anything)
* `<Regexp>*` is the closure (i.e., match zero or more)
* `<Regexp> <Regexp>` is concatenation (e.g., `P I` matches one parent edge followed by an
  import)
* `<Regexp> | <Regexp>` is a logical or (e.g., `P | I` matches a parent or an import edge)
* `<Regexp> & <Regexp>` is a logical and (i.e., both expressions must match)
* `(<Regexp>)` brackets can be used for grouping

Match specifies which data in the relation we want to match.
E.g., to match on a name `x`, the match is an anonymous rule `{ d :- d == Var{x@_} }` which is tested against all the declarations `d` that are reachable.

The label order ...

The shadowing determines which declarations shadow each other.
If it is `true`, all declarations shadow each other, and we only get the declarations reached via the shortest path.
If `false`, none, which could be used to find all reachable declarations.
Or, something like `{ Var{x@_}, Var{y@_} }` specifies that we only shadow between things with the same name.

