---
layout: page
title: "Lecture 7: Type Checking"
excerpt: "Type Checking"
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

- [PDF 2018](https://github.com/TUDelft-CS4200-2019/lectures/raw/master/07-type-checking/CS4200-2018-7-type-checking.pdf)
- [PDF 2019](https://github.com/TUDelft-CS4200-2019/lectures/raw/master/07-type-checking/CS4200-2019-7-type-checking.pdf)
- [PDF 2019 (w/ animations)](https://github.com/TUDelft-CS4200-2019/lectures/raw/master/07-type-checking/CS4200-2019-7-type-checking-animated.pdf)
- [github](https://github.com/TUDelft-CS4200-2019/lectures/tree/master/07-type-checking)

<iframe src="//www.slideshare.net/slideshow/embed_code/key/xxXCCYh8Iwi2jI" width="595" height="485" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC; border-width:1px; margin-bottom:5px; max-width: 100%;" allowfullscreen> </iframe> <div style="margin-bottom:5px"> <strong> <a href="//www.slideshare.net/eelcovisser/compiler-construction-lecture-7-type-checking" title="Compiler Construction | Lecture 7 | Type Checking" target="_blank">Compiler Construction | Lecture 7 | Type Checking</a> </strong> from <strong><a href="https://www.slideshare.net/eelcovisser" target="_blank">Eelco Visser</a></strong> </div>


### Reading Material

- Gabriël Konat, Lennart C. L. Kats, Guido Wachsmuth, Eelco Visser. [Declarative Name Binding and Scope Rules](http://dx.doi.org/10.1007/978-3-642-36089-3_18). In Krzysztof Czarnecki, Görel Hedin, editors, Software Language Engineering, 5th International Conference, SLE 2012, Dresden, Germany, September 26-28, 2012, Revised Selected Papers. Volume 7745 of Lecture Notes in Computer Science, pages 311-331, Springer, 2012. This paper introduces the NaBL language for describing the name binding rules of programming languages. This work inspired the development of the scope graph model.

- Pierre Néron, Andrew P. Tolmach, Eelco Visser, Guido Wachsmuth. [A Theory of Name Resolution](http://dx.doi.org/10.1007/978-3-662-46669-8_9). In Jan Vitek, editor, Programming Languages and Systems - 24th European Symposium on Programming, ESOP 2015, Held as Part of the European Joint Conferences on Theory and Practice of Software, ETAPS 2015, London, UK, April 11-18, 2015. Proceedings. Volume 9032 of Lecture Notes in Computer Science, pages 205-231, Springer, 2015. This paper introduces the scope graph model for the representation of name binding facts in programs and the resolution calculus to resolve names in scope graphs.

- Hendrik van Antwerpen, Pierre Néron, Andrew P. Tolmach, Eelco Visser, Guido Wachsmuth. [A constraint language for static semantic analysis based on scope graphs](http://doi.acm.org/10.1145/2847538.2847543). In Martin Erwig, Tiark Rompf, editors, Proceedings of the 2016 ACM SIGPLAN Workshop on Partial Evaluation and Program Manipulation, PEPM 2016, St. Petersburg, FL, USA, January 20 - 22, 2016. pages 49-60, ACM, 2016. This paper develops a constraint language for description of type system using scope graph constraints for the definition of name binding. These constraints are the basis for the NaBL2 language.

- [Static Semantics with NaBL2](http://www.metaborg.org/en/latest/source/langdev/meta/lang/nabl2/index.html). Documentation
