---
layout: page
title: "Lecture 8: Constraint Semantics & Resolution"
excerpt: "Constraint Semantics & Resolution"
tags: ["lecture"]
context: lectures
subcontext: statics
# image:
#    feature: "lecture.jpg"
#    credit: Delft University of Technology
#    creditlink: http://repository.tudelft.nl/view/MMP/uuid%3Aa2f25709-c56e-453e-9394-4a05acf603a4/
---

In this lecture we study constraint-based static semantic analysis using the scope graph framework for name resolution.


## Slides

- [PDF](https://github.com/TUDelft-CS4200-2019/lectures/raw/master/08-type-constraints/CS4200-2019-8-type-constraints.pdf)
- [PDF 2018 (1)](https://github.com/TUDelft-CS4200-2019/lectures/raw/master/08-type-constraints/CS4200-2018-8-type-constraints.pdf)
- [PDF 2018 (2)](https://github.com/TUDelft-CS4200-2019/lectures/raw/master/08-type-constraints/CS4200-2018-9-constraint-resolution.pdf)
- [github](https://github.com/TUDelft-CS4200-2019/lectures/tree/master/08-type-constraints)

<iframe src="//www.slideshare.net/slideshow/embed_code/key/DVvLXFixwJZuPw" width="595" height="485" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC; border-width:1px; margin-bottom:5px; max-width: 100%;" allowfullscreen> </iframe> <div style="margin-bottom:5px"> <strong> <a href="//www.slideshare.net/eelcovisser/compiler-construction-lecture-8-type-constraints" title="Compiler Construction | Lecture 8 | Type Constraints" target="_blank">Compiler Construction | Lecture 8 | Type Constraints</a> </strong> from <strong><a href="https://www.slideshare.net/eelcovisser" target="_blank">Eelco Visser</a></strong> </div>

<iframe src="//www.slideshare.net/slideshow/embed_code/key/lbV0WwZoC6iUQh" width="595" height="485" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC; border-width:1px; margin-bottom:5px; max-width: 100%;" allowfullscreen> </iframe> <div style="margin-bottom:5px"> <strong> <a href="//www.slideshare.net/eelcovisser/compiler-construction-lecture-9-constraint-resolution" title="Compiler Construction | Lecture 9 | Constraint Resolution" target="_blank">Compiler Construction | Lecture 9 | Constraint Resolution</a> </strong> from <strong><a href="https://www.slideshare.net/eelcovisser" target="_blank">Eelco Visser</a></strong> </div>

### Reading Material

- Hendrik van Antwerpen, Casper Bach Poulsen, Arjen Rouvoet, Eelco Visser. [Scopes as types](https://doi.org/10.1145/3276484). Proceedings of the ACM on Programming Languages, 2 (OOPSLA), 2018. This paper introduces Statix, a language for writing static semantics specifications.

- Pierre Néron, Andrew P. Tolmach, Eelco Visser, Guido Wachsmuth. [A Theory of Name Resolution](http://dx.doi.org/10.1007/978-3-662-46669-8_9). In Jan Vitek, editor, Programming Languages and Systems - 24th European Symposium on Programming, ESOP 2015, Held as Part of the European Joint Conferences on Theory and Practice of Software, ETAPS 2015, London, UK, April 11-18, 2015. Proceedings. Volume 9032 of Lecture Notes in Computer Science, pages 205-231, Springer, 2015. This paper introduces the scope graph model for the representation of name binding facts in programs and the resolution calculus to resolve names in scope graphs.

- Baader, Franz, Wayne Snyder, Paliath Narendran, Manfred Schmidt-Schauss, and Klaus Schulz. [“Chapter 8 - Unification Theory.”](https://www.cs.bu.edu/~snyder/publications/UnifChapter.pdf) In Handbook of Automated Reasoning, edited by Alan Robinson and Andrei Voronkov, 445–533. Handbook of Automated Reasoning. Amsterdam: North-Holland, 2001. https://doi.org/10.1016/B978-044450813-3/50010-2. This is a good introduction to term unification. The course material is covered in §1 and §2.

### Additional Reading Material

- Hendrik van Antwerpen, Pierre Néron, Andrew P. Tolmach, Eelco Visser, Guido Wachsmuth. [A constraint language for static semantic analysis based on scope graphs](http://doi.acm.org/10.1145/2847538.2847543). In Martin Erwig, Tiark Rompf, editors, Proceedings of the 2016 ACM SIGPLAN Workshop on Partial Evaluation and Program Manipulation, PEPM 2016, St. Petersburg, FL, USA, January 20 - 22, 2016. pages 49-60, ACM, 2016. This paper develops a constraint language for description of type system using scope graph constraints for the definition of name binding. These constraints are the basis for the NaBL2 language.

- Gabriël Konat, Lennart C. L. Kats, Guido Wachsmuth, Eelco Visser. [Declarative Name Binding and Scope Rules](http://dx.doi.org/10.1007/978-3-642-36089-3_18). In Krzysztof Czarnecki, Görel Hedin, editors, Software Language Engineering, 5th International Conference, SLE 2012, Dresden, Germany, September 26-28, 2012, Revised Selected Papers. Volume 7745 of Lecture Notes in Computer Science, pages 311-331, Springer, 2012. This paper introduces the NaBL language for describing the name binding rules of programming languages. This work inspired the development of the scope graph model.
