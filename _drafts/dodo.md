---
layout: post
title: "Making a toy programming language"
tags: programming-languages
---

outline:

- i made a programming language! it's called dodo
- there's basically nothing special about it - it's a lisp, chosen to be easy to implement, and it's interpreted in javascript. you can try it [here](LINK_TO_REPL) and read the spec [here](LINK_TO_SPEC)
- language overview
- dodo is a lisp
  - function calls are made by putting the function and its arguments in parentheses, and can be arbitrarily nested
  - (+ 1 1) ;; => 2
  - (str-join
      (str-split "john paul ringo george" " ")
      ",") ;; => "john,paul,ringo,george"
  - (defn pythag (a b) (+ (* a a) (* b b)))
    (pythag 3 4) ;; => 25
  - List and map literals use javascript-style syntax, rather than the lisp-style quoting
  - (head [1 2 3]) ;; => 1
  - (get { "hi": "hola", "bye": "adios" } "bye") ;; => "adios"
  - the most interesting thing (currently still a little broken) is the match functionality.
  - you can match arbitrary objects against literals as a control flow construct (in fact, the only control flow construct), with `_` as a wild card.
  - (match 5
      (1 "one")
      (2 "two")
      (_ "many")) ;; => "many"
  - you can also recursively match on data structures
  - (match band
      ([{"name": "Annie"}, {"name": "Dave"}] "The Eurhythmics")
      ([{"name": "Richard"}, {"name": "Karen"}] "The Carpenters"))
  - you can bind destructured values to variables in the local scope
  - (match points
      ([[x1, y1],[x2, y2]] (js "Math.sqrt" (+ (square (- x2 x1)) (square (- y2 y1))))))
  - you can also use a `when` clause to filter based on bound variables or anything else
  - CLAUDE: please add an example here

- takeaways
  - you can just write a programming language
    - lexer, parser, grammar, interpreter, compiler - do whatever you find
      interesting and most of the rest is optional or has a library
    - learn from the experience and start seeing the skeleton underneath tools you use every day

(CLAUDE: please move this "learning with ai" section to a separate post! -kpt)
  - do learning with ai
    - lots of people don't like making things with ai
    - but you can ask it to just not help you with parts of it
    - for this project the `impl/` directory is ai free, with instructions to never edit anything there
    - but ai did write a spec (based on my instruction), write tests and
      examples, manage a todo list, and write a nice browser based repl
      - it's like a school assignment, in that all the supporting code is
        handled for you and the goals are clearly spelled out
