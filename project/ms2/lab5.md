---
layout: page
title: "Lab 5: Name Analysis"
excerpt: "Lab 5: Name Analysis"
tags: ["project"]
context: project
subcontext: ms2
---

{% include _toc.html %}

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
* constraints (55 points)
  * unresolved references (16 points)
  * duplicate definitions (20 points)
  * hiding variables and fields (9 points)
  * overriding (10 points)

### Early Feedback

We provide early feedback for your language implementation.
This feedback gives you an indication which parts of your implementation might still be wrong.
It includes a summary on how many tests you pass and how many points you earn by passing them.
You have 3 early feedback attempts.

## Detailed Instructions

### Preliminaries

For this lab you need to update Spoofax to a new version.
Follow the instructions in the [Spoofax documentation](/documentation/spoofax.html#updating).
{: .notice .notice-warning}

#### GitLab Repository

You continue with your work from the previous assignment. See the
[Git documentation](/documentation/git.html#continue-from-previous-assignment) on
how to create the `assignment-5-develop` branch from your previous work.

### Example specification

A complete example specification for the simply typed lambda calculus can be found at [STLC](lab5-example.html).

### Name binding rules

Name binding is specified through name binding and typing rules in a `.stx` file. Statix files must go in
the `trans/analysis` directory. The module name at the top of the file should match the filename relative to `trans`. For example, the file `trans/analysis/minijava.stx` starts as:

```
module analysis/minijava
```

#### Predicates
Type checking is specified using predicates whose rules determine which programs are well-formed and which are not.
The rules typically match on the syntactic constructs of the language.
The rule body specifies the premises that must hold for the construct to be well-formed.
Each predicate is has a signature that describes the types of the input arguments.
Predicates and rules are defined in the `rules` section of the specification.
For example, the signature of the predicate that checks expressions together with two of its rules looks like this:

```
rules

   expOk : scope * Exp                           
   // signature for predicate with two parameters
   
   expOk(s, Call(e, x, args)) :- {y z} PREMISES. 
   // rule for the case Call (end in a dot!)

   expOk(s, True()).                             
   // rule for the case True
```

The signature specifies that the predicate `expOk` takes two parameters.
The first argument must be of the built-in type `scope`, the second of the type `Exp`, which comes from the signature of the language.
This example shows two forms for predicate rules.
The first (for `Call`) is the full form, matches on the `Call` construct in the head of the rule (the part before `:-`), defines local variable `y` and `z`, followed by a body containing comma-separated premises.
The second is an abbreviated form for rules that have no premises.
Any variables that appear in the head are bound in the body of the rule.
Furthermore, extra local variables are declared between curly braces at the beginning of the body.

Rules of one predicate must not overlap, that is, they must not match on the same constructs as other rules for the predicate.
Overlapping rules are statically detected and Statix gives an error on those.
{: .notice .notice-warning}

There are several *premises* (also called constraints) that can be part of the rule body.
The simplest are `true` (which always succeeds) and `false`, which never succeeds.
For example, the abbreviated rule is equal to `expOk(s, True()) :- true.`

It is also possible to use predicates as premises.
For example, the `Call` rule may enforce well-formedness of the nested expression by having a premise `expOk(s, e)` using predicate `expOk`.

The initial project contains a predicate `programOk` with a rule that matches the root AST node of a Mini Java program.
Type checking proceeds from this rule by matching rules against arguments and unfolding to the premises of matching rules.
If all premises can be resolved, the program is considered well-formed.
The predicate that is invoked first in the analysis is specified in the 
`trans/analysis.str` Stratego file which defines a strategy `str-editor-analyze`.

```
editor-analyze = stx-editor-analyze(
  explicate-injections;desugar-all
  |"statics", "programOk")
```

#### Grammar injections
A grammar injection (e.g. `Exp = VarRef`) includes all the productions of the right-hand side sort (here `VarRef`) as valid for the left-hand side sort (here `Exp`) without having to wrap them in a named constructor. However, to satisfy the requirements of Statix type checking, auxiliary constructors are introduced that make these injections explicit. 

For example, the above injection (`Exp = VarRef`) is represented by `VarRef -> Exp` in the following snippet of MiniJava signature for expressions in Stratego: 

``` 
/* Stratego signature */
signature
  sorts
    Exp VarRef ID

  constructors
    VarRef   : ID -> VarRef
             : VarRef -> Exp  // injection
```

but represented with an explicit constructor `VR2E` in the Statix specification:

```
/* Statix signature */
signature // expressions
  sorts
    Exp VarRef IndexExp

  constructors
    VarRef    : ID -> VarRef
    VR2E      : VarRef -> Exp  // explicit injection constructor
```

For this lab you have to write predicate rules for *all* the constructs in the 
language. If constructs are left uncovered by rules and then encountered in a 
MiniJava program, Statix analysis will fail.
This means you have to explicitly match on the injection constructors. 
All such injection constructors you have to use are mentioned in the signature 
of the template Statix file.

You can use the `Spoofax > Show AST` or `Spoofax > Show desugared AST` on a MiniJava file to see the the explicit injection constructors in the AST.
{: .notice .notice-info}

Check the signature provided in the Statix file of the template project to 
make sure you write rules for all the language constructs.
{: .notice .notice-warning}

<!--### Terms and Sorts

AST, names, types are all terms, Statix is fully typed**

... term and pattern syntax ...

... sorts ... -->

### Name resolution

<!--Modelling name binding in Statix involves the following tasks:
- Creating scopes and connecting them with appropriate edges
- Adding type, method, field, and variable declarations in the scope graph
- Resolving type, field, and variable references and connecting resolution results to the AST
- More sophisticated checks like overriding, duplicate names, hiding-->

#### Scopes and Edges

A scope graph is constructed from scopes and edges using the predicates:

* `new s` that introduces a new scope and
* `s -l-> s'` that links a scope `s` to existing scope `s'` with an edge labelled `l'`

To distinguish between different kinds of scope graph edges, the edge labels 
must be defined in the signature as a space-separated list:

```
signature
  name-resolution
    labels
```

#### Namespaces

<!--**Add declarations for the different kinds of names**-->
Different kinds of names can be kept separate by putting them in a different *namespace*. For example, variables live in the Var namespace. A name with a namespace and an (implicit) AST position is called an *occurrence*, and written as Var{x}, where Var is the namespace, and x the name.

Namespaces are declared in the signature and specify the type of the name.
In this lab, the names should always be `ID`s, but in general they can be arbitrary terms.

```
signature
  namespaces
    Var : ID
```

#### Declarations

Declarations are added to the scope graph using `s -> OCCURRENCE`.
This adds it to the implicit relation `decl : occurrence`.
Other relations may be defined if additional information has to be associated with the occurrence.
For example, one could define a relation that associates an occurrence with a scope:

```
signature
  relations
    scopeOf : occurrence -> scope
```

In this case, a declaration is then defined using `s -> OCCURRENCE with scopeOf SCOPE`.

### Constraints
#### Name resolution

A reference in the scope graph must resolve to a declaration. This is specified with a premise in the shape of a scope graph query. 
To query for declarations (in the implicit `decl` relation), queries can be formulated as `OCCURRENCE in s |-> ps` which resolve an occurence in scope `s` to `ps`. The latter is a list of pairs of `path`s and the data from the relation (occurrences in this case). 

As usual, the list of results `ps` can be pattern-matched on. For example, to match on a single variable declaration you could write `Var{x} in s |-> [(_, Var{x'})]`, where `x'` is bound to the name of the original declaration.
<!--**Checking duplicate definitions**-->

Queries that resolve in another relation instead (e.g. for `scopeOf` defined above) can be constructed like `relation-name of OCCURRENCE in s |-> ps`.

The well-formedness and disambiguation of resolution paths can be controlled per namespace and are defined in the signature for *name-resolution*:

```
signature
  name-resolution
    resolve Ns filter REGEX min LABELORDER // Ns is namespace, e.g. Var
```

The well-formedness is a regular expression `REGEX` on path labels which rules out paths that do not match
it. The regular expressions on labels have the following syntax:

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

The default case `P* I*` means that after an import, 
you cannot look to resolve in a parent scope anymore. 

To prioritise and disambiguate between multiple well-formed resolution paths we define a `specificity ordering` of paths by giving an ordering of edge labels. `LABELORD` is thus a space-separated list of `label1 < label2`. For example, `$ < P` indicates that declaration edges are preferred to parent edges `P`.

#### Custom errors and warnings

It is possible to control the errors and warnings that are generated if a constraint fails.
For equalities, resolution constraints and queries, `false`, and predicates, an error can be specified as:
```
constraint | error <Message> <Location>
```

The message is optional and can be

1. a literal string `"variable not found"`, or 
2. a formatted message `$[Cannot find variable [x]]`. 

The message text itself is currently ignored in Spoofax. This will be fixed soon.
{: .notice .notice-warning}

The default error location is the term matched with the rule, but it can be specified explicitly using `@t`, where `t` is a variable from your rule.

There is a special construct to produce warnings and notes:
```
try { constraint } | <Severity> <Message> <Location>
```

The severity is `error`, `warning` or `note`.
The `try` constraint tests if the embedded constraint holds in the solution of the original constraint problem.
However, it cannot influence that solution.
This ensures that the constraints that produce a warning cannot accidentally cause an error because they instantiated constraint variables.

#### Hiding and overriding

MiniJava requires errors in a few cases where *hiding* occurs. Fields are not allowed to
hide fields in super classes. Parameters are not allowed to hide fields, but they get a warning 
if they do. MiniJava does not allow a parameter to have the same name as
a local variable in a method.

Furthermore, in Lab 7 we need to test that an overriding method has the same type as the overridden method in the
parent class. Therefore, we now want to introduce a premise describing the logic of the overriding method resolving to its
overridden parent method. For example, consider the following situation:

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

Here the declaration of `Bar.m()` should reference the declaration of `Foo.m()`.

To formulate appropriate premises that describe this behaviour, we can write 
longer, full-form queries which allow us to ask more powerful questions. 
The full query format to query a Statix scope graph is then:

```
query RELATION filter REGEX and MATCH min LABELORD and SHADOW in SCOPE |-> RESULT
```

The shorter resolution queries from the previous section are all explicated 
to such full queries.

`RELATION` can be a custom relation name, `decl`, or `()` which only looks at the reachable scopes.

As seen before, `REGEX` specifies path well-formedness and `LABELORD` determines the edge label order. Both can override the default specification defined for a namespace.

`MATCH` specifies which data in the relation we want to match.
For example, to match on a name `x`, the match is an anonymous rule `{ Var{x'} :- x' == x }` which is tested against all the declarations `d` that are reachable.

`SHADOW` determines which declarations shadow each other.
If it is set to `true`, all declarations shadow each other and we only get the declarations reached via the shortest path.
If set to `false`, none of the declarations shadow which could be used to find all reachable declarations.
Alternatively, an anonymous rule like `{ Var{x}, Var{x} }` could be used to specify we only shadow between things with the same name (and within the same namespace).

To see the full forms of the shorter queries, 
you can use `Syntax > Format Normalized` on the Statix specification.
{: .notice .notice-info}

#### Editor Resolution

<!--**Connecting resolution to the AST**-->

To enable name resolution as a service in the editor, we have to link references to the desired declarations.
This is achieved by defining the `ref` property on AST nodes using the syntax `x.ref := x'`, where `x` is the reference name and `x'` the name matched from the desired declaration in the query result.

