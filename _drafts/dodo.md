---
layout: post
title: "Making a toy programming language"
tags: programming-languages
---

I made a programming language! It's called dodo. There's basically nothing special about it — it's a Lisp, chosen to be easy to implement, and it's interpreted in JavaScript. You can try it [here](/dodo/repl.html) and read the spec [here](/dodo/spec.html).

## The language

Dodo is a Lisp. Function calls are made by putting the function and its arguments in parentheses, and can be arbitrarily nested:

```
(+ 1 1) ;; => 2
```

```
(str-join
    (str-split "john paul ringo george" " ")
    ",") ;; => "john,paul,ringo,george"
```

```
(defn square (x) (* x x))
(defn pythag (a b) (+ (square a) (square b)))
(pythag 3 4) ;; => 25
```

List and map literals use JavaScript-style syntax, rather than Lisp-style quoting:

```
(head [1 2 3]) ;; => 1
(get { "hi": "hola", "bye": "adios" } "bye") ;; => "adios"
```

The most interesting thing (currently still a little broken) is the match functionality. You can match arbitrary objects against literals as a control flow construct (in fact, the only control flow construct), with `_` as a wildcard:

```
(match 5
    (1 "one")
    (2 "two")
    (_ "many")) ;; => "many"
```

You can also recursively match on data structures:

```
(match band
    ([{"name": "Annie"}, {"name": "Dave"}] "The Eurhythmics")
    ([{"name": "Richard"}, {"name": "Karen"}] "The Carpenters"))
```

You can bind destructured values to variables in the local scope:

```
(match points
    ([[x1, y1],[x2, y2]] (js "Math.sqrt" (+ (square (- x2 x1)) (square (- y2 y1))))))
```

You can also use a `when` clause to filter based on bound variables or anything else:

```
(match n
    (x when (> x 0) "positive")
    (x when (< x 0) "negative")
    (_ "zero"))
```

## Takeaways

You can just write a programming language. Lexer, parser, grammar, interpreter, compiler — do whatever you find interesting and most of the rest is optional or has a library. Learn from the experience and start seeing the skeleton underneath the tools you use every day.
