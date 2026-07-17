---
layout: post
title: "Solving a corn puzzle with CP-SAT"
tags:
---

One random Friday at [RC](recurse.com), I was bored and playing around with a
puzzle someone had 3d printed:

<img src="/assets/images/corn-puzzle/corn-4.jpg" alt="corn puzzle" width="400">

It's a physical puzzle consisting of a white "cob" with grooves in it and a
bunch of "corn" pieces, each made of 3-10 kernels stuck together in a particular
shape, that can slide into the grooves. The goal is to slide them on so that
they fit together exactly, with no overlaps and no gaps.

Like all software engineers in the year of our lord 2026, I quickly got tired of
solving the puzzle myself and asked Claude to do it for me:

<img src="/assets/images/corn-puzzle/claude-prompt.png" alt="my prompt to Claude" width="600">

After a little bit of wrangling - it turns out computer vision isn't *quite*
that good yet, so I had to clarify some of the piece shapes - Claude wrote a
script in python that gave a solution.

It was much better than I would have written myself, and I ended up learning
from it.

<img src="/assets/images/corn-puzzle/corn-9.jpg" alt="corn puzzle" width="400">

### What I expected

If I was solving this problem myself, I would naively reach for recursive
backtracking. If you're a programmer, you've probably seen this algorithm in an
intro CS class.

Basically, it's the brute force approach - try something at random, then try
something else, keep going until the puzzle is solved. If you ever get stuck and
there are no legal moves left, undo your last move and try again from there. If
all moves from that point fail, undo another move, and so on.

This is just an organized way of trying all the options, and it does admit some
nice optimizations based on the structure of the problem. For example, you can
take into account the symmetry that rotating the cob doesn't change the solution
in any way, or fail early if you ever isolate a single kernel space on its own
(so that no piece can cover it). So a well thought-out approach to backtracking
would likely work.

However, when I looked at Claude's script I realized from the first line that I
was being dumb:

```python
from ortools.sat.python import cp_model
```

### This wheel has already been invented

Instead of backtracking, Claude just imported an industrial-strength library
made to solve these sorts of problems. [OR-Tools
CP-SAT](https://developers.google.com/optimization/cp/cp_solver) is put out by
Google and is made for solving constrained optimization problems, as well as
satisfiability problems like this one.

The idea here is to model any such problem as a set of variables - classically
binary - and a set of constraints or assertions about those variables that all
have to be simultaneously satisfied. (As well as an objective function, for
optimization problems.) Then the library draws on many decades of research to
efficiently find a setting for those variables that meets all of the
constraints.

The corn problem is a variant of the [Exact
Cover](https://en.wikipedia.org/wiki/Exact_cover) problem, so those in the know
can probably already see how it would be represented, but I'll go through it as
it was new to me.

The variables in this problem are binary - true or false - and each one
represents the idea "X piece is placed in a particular way". So for each of
$N_i$ ways that piece $i$ could be placed, we have one variable $V_{i,n}$, which
is true ($1$) if the piece is placed there, false ($0$) if it isn't.

__Constraint class 1: All pieces are placed exactly once__

This part is easy - for each piece $i$, we have

$ \sum_{n=1}^{N_i} V_{i,n} = 1 $

This enforces that exactly one of the $$V_{i, n}$$ variables is $1$, and the
rest are $0$.

__Constraint class 2: Each spot is covered exactly once__

This is a little harder, but just tedious, not confusing. For each piece
placement we have to go through all of the spaces on the cob that it covers and
add it to the set of covering pieces $C_s$. Then for all spaces $s$ on the cob,
we have a very similar looking constraint:

$ \sum_{V_{i,n} \in C_s} V_{i,n} = 1 $

So every piece is used once, and every spot is covered once. If we assume that
this is a well constructed puzzle - so the total number of kernels in the pieces
is equal to the number of spaces on the cob - this is all the constraints we
need!

Claude's python script to solve this was pretty complicated because it had to
actually enumerate all the possible positions of all the pieces, but actually
invoking the solver was very straightforward:

```python
    for plist in by_piece:
        m.Add(sum(v for v, _ in plist) == 1)
    for clist in by_cell:
        m.Add(sum(clist) == 1)
 
    # ...
    
    s.solve(m)
```

### An artisanal hand-crafted CP-SAT invocation

To learn more about this and try to lock in the lesson not to reinvent the
wheel, I got together with [Zaki](https://github.com/zmughal) and
[Tommy](tommymaranges.com) at recurse to do what is probably the most classic
application of cp-sat to a puzzle, Sudoku.

This is a classic beginner application of the tool and easier to apply than for
the corn puzzle; we simply have one variable per cell, with a value 1-9, and our
constraint classes are that every row, column, and box has to have one of each.
We don't need to have utility methods for setting this up, as the corn puzzle
did for rotating pieces:

```python
  model_vars = [ [ None for _ in range(9) ] for _ in range(9) ]
  for i in range(9):
    for j in range(9):
      board_var_name = f"s[{i},{j}]"
      if board[i][j] == -1:
        model_vars[i][j] = model.new_int_var(1, 9, board_var_name)
      else:
        # for given values, there's only one legal value for the variable
        model_vars[i][j] = model.new_int_var(board[i][j], board[i][j], board_var_name)

  # rows
  for r in range(9):
    model.add_all_different( [ model_vars[r][c] for c in range(9) ] )

  # columns
  for c in range(9):
    model.add_all_different( [ model_vars[r][c] for r in range(9) ] )

  # boxes
  for off_r in range(3):
    for off_c in range(3):
      model.add_all_different( [
          model_vars[3*off_r + numpad % 3][3*off_c + numpad // 3]
            for numpad in range(9) ] )
```

This indeed solved a reference sudoku we gave it, quickly.

There's a lot more left to learn about these, but we've now learned the most
important thing - when I get another cp-sat shaped problem in the future, I'll
know to try a solver and read the docs for whatever else I need, rather than
roll my own.

#### Links

[The conversation with claude about solving the corn puzzle (with claude's python script)](https://claude.ai/share/37b27e39-f11b-4a9a-98b0-04b98fa84662)

[Our sudoku solver](https://github.com/kpthill/cp-sudoku)

[Slides from a short talk I gave about this at RC](https://docs.google.com/presentation/d/17L0fYkLVCgGt8L1rCSSJ3ymXYsKTvNMIRRTmK69V5A8/edit?usp=sharing)
