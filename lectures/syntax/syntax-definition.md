---
layout: page
title: "Lecture 2: Declarative Syntax Definition"
excerpt: "Declarative Syntax Definition"
tags: ["lectures"]
context: lectures
subcontext: syntax
# image:
#    feature: "lecture.jpg"
#    credit: Delft University of Technology
#    creditlink: http://repository.tudelft.nl/view/MMP/uuid%3Aa2f25709-c56e-453e-9394-4a05acf603a4/
---

In this lecture we study declarative syntax definition, i.e. syntax definition that focuses on the definition of the structure (abstract syntax) and notation (concrete syntax) of programs, and abstracts from the implementation of parsers.

In this lecture we take a further look at declarative syntax definition, with the specification of lexical syntax and the desugaring of context-free syntax and lexical syntax into a core grammar formalism.

### Topics

  - program structure, syntactic categories, language constructs
  - abstract syntax, signatures, sorts, constructors
  - context-free grammars
  - concrete syntax, notation
  - lexical syntax, literals, keywords
  - ambiguity, disambiguation, associativity, priority

### Slides

  - [PDF 2019](https://github.com/TUDelft-CS4200-2019/lectures/raw/master/02-syntax-definition/CS4200-2019-2-syntax-definition.pdf)
  - on [github](https://github.com/TUDelft-CS4200-2019/lectures/tree/master/02-syntax-definition)

<iframe src="//www.slideshare.net/slideshow/embed_code/key/yGYmMyKVY89b80" width="595" height="485" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC; border-width:1px; margin-bottom:5px; max-width: 100%;" allowfullscreen> </iframe> <div style="margin-bottom:5px"> <strong> <a href="//www.slideshare.net/eelcovisser/cs4200-2019-lecture-2-syntaxdefinition" title="CS4200 2019 | Lecture 2 | syntax-definition" target="_blank">CS4200 2019 | Lecture 2 | syntax-definition</a> </strong> from <strong><a href="https://www.slideshare.net/eelcovisser" target="_blank">Eelco Visser</a></strong> </div>

### Reading material

* Lennart C. L. Kats, Eelco Visser, Guido Wachsmuth. [Pure and declarative syntax definition: paradise lost and regained.](https://doi.org/10.1145/1932682.1869535) In William R. Cook, Siobhán Clarke, Martin C. Rinard, editors, Proceedings of the 25th Annual ACM SIGPLAN Conference on Object-Oriented Programming, Systems, Languages, and Applications, OOPSLA 2010, October 17-21, 2010, Reno/Tahoe, Nevada, USA. pages 918-932, ACM, Reno/Tahoe, Nevada, 2010. <https://doi.org/10.1145/1932682.1869535>

* Lennart C. L. Kats, Rob Vermaas, Eelco Visser. [Integrated language definition testing: enabling test-driven language development.](https://doi.org/10.1145/2076021.2048080) In Cristina Videira Lopes, Kathleen Fisher, editors, Proceedings of the 26th Annual ACM SIGPLAN Conference on Object-Oriented Programming, Systems, Languages, and Applications, OOPSLA 2011, part of SPLASH 2011, Portland, OR, USA, October 22 - 27, 2011. pages 139-154, ACM, 2011. <https://doi.org/10.1145/2076021.2048080>

* [Syntax Definition with SDF3](http://www.metaborg.org/en/latest/source/langdev/meta/lang/sdf3/index.html). Documentation of the SDF3 syntax definition formalism
