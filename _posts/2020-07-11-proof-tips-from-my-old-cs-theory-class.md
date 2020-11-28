---
layout: post
title:  "Proof Tips from my Old CS Theory Class"
tags:   math
---

Back in the early 20-teens I was a TA for CS103, Stanford's introduction to CS theory. For many of the students it was their first introduction to proof-based mathematics. We started off with an introduction to propositional logic, with two-column proofs and very explicit rules about how everything was supposed to work.

The students took to this pretty quickly, and unsurprisingly so - the rigid formalism of propositional logic maps pretty closely onto the rigid formalism of programming languages that they were already familiar with. However, a little less than halfway through the quarter, we switched to expecting normal paragraph-style proofs such as you might see in published papers, and the students started having a ton of trouble.

They would leave out essential justifications, or include extraneous ones. They would meander around the point without ever reaching it. They would appeal to vague concepts, like the "arrow from x values to y values" definition of functions, rather than engaging with the concrete definitions. They would claim things "by definition" that had no relation to the definitions given in the book.

I wrote the following handout to help them understand how to write cleaner, more rigorous proofs. It never actually got used - they figured out what they were doing well before I figured out what I wanted to say - but I include it here in hopes that you might find it interesting.

## CS103 Proof Tips

Every proof in this class should be an informal, "paragraph style" proof from here on out unless the problem says otherwise.  We’ve had a lot of experience with formal proofs up till now, but informal proofs have their own style and their own requirements.  Paragraph style proofs are what will be expected in every theory or math class that you might take, and are how proofs are written in the literature, so it is worth getting familiar with the way that they are written.

Informal proofs should all be in one or more paragraphs.  The lines should not be separated or numbered, and justifications of boolean logic or simple arithmetic may be skipped. Because of this, informal proofs look very different from formal proofs.  However, they are very closely related.  A mathematically competent reader should be able to construct a formal proof from an informal proof, more or less mechanically - without having to have any deep thoughts or creative insights.  As a result, even in an informal proof you have to do more than just convey the intuitions behind a problem - you have to actually address the definitions that govern the thing you are trying to prove.

In particular, if the definition of something that you are trying to prove includes a (universal or existential) quantifier, you do actually need to instantiate that quantifier - even in an informal proof!  This means that you should be talking about a specific object, with a name.  An “x” or an “a” or a “p”, but not just "that element".  This will make it much easier on the readers of your proof, and much easier for you to keep track of what is going on when you construct it.

### Instantiation and Generalization

Remember from when we were doing propositional logic with quantifiers and we had to make a distinction between “universal” and “existential” quantifiers?  This distinction made a big difference in what we were allowed to do in a formal proof; even now that we are doing informal proofs, this distinction is still very important!  For example, let’s try proving a very simple theorem:

_Theorem 1_ There is some positive number which divides every positive number.

By writing this in the language of formal logic, we can get a sense of how our proof should look:

_Theorem 1_ $$ \exists x \in \mathbb{N} s.t. \forall y \in \mathbb{N}, x \vert y $$

“There exists a natural number x such that for every natural number y, x divides y.”

Aha! From the first week of the course, we know how to write a formal proof of this - first we have to pick some x, then pick an arbitrary y, then prove that x divides y, then use universal generalization to conclude that for all y, x divides y, and then use existential generalization to say that, because this x works, there is some x that works.  Similarly for an informal proof, we may not know how to prove this, but we at least know how the proof will look:

_Proof (template)_

Pick $$ x = \_\_\_\_$$. Then $$ x \in \mathbb{N} $$ because \_\_\_\_. Let $$y \in \mathbb{N}$$.

{Now, prove x divides y.}

Therefore, x divides y, and we are done.

Notice how we haven’t even _done anything_ yet.  This proof structure follows immediately from the structure of the theorem - to show that there exists an x, we can pick any x we want, as long as it is a natural number, and then to show something is true for every y, we have to let y be arbitrary, and then we need to prove that x divides y.  Once we do this, because of the way we set up the proof, we have shown the theorem - we can skip the generalization steps at the end, because they are fixed by what we did at the beginning (which means you have to be very careful with the set up!).  The other thing to note is that the order is important - $$ \exists x \forall y $$ does not mean the same thing as $$ \forall y \exists x $$, and so we need to be certain that we pick x before we let y, or we will have proved something completely different!

How do you prove that x divides y?  Well, let’s just look at the definition:

_Definition_ We say that “x divides y” or $$ x \vert y $$ if and only if there exists some natural number n such that $$ y = n x $$. That is, $$ x \vert y \iff \exists n \! \in \! \mathbb{N} \, s.t. \, y = n x $$.

Well, we still may not know why the theorem is true, but now we can see another step that the eventual proof is going to have to have, within the {prove x divides y} section:

Pick $$n = \_\_\_\_$$.  Then $$ n \in \mathbb{N} $$ because \_\_\_\_.

{ Prove $$y=n x$$. }

Therefore x divides y.

Now we still haven’t done anything interesting - we are just chasing definitions.  But looking over the proof skeleton, it is pretty easy to guess that $$ x = 1 $$ and $$ n = y $$, so we can fill in the blanks:

_Proof_

Pick $$ x=1 $$, so $$ x \in \mathbb{N} $$.  Let $$ y \in \mathbb{N} $$. Pick $$ n = y $$, and hence $$ n \in \mathbb{N} $$ as well.  Then

$$n*x=y*1=y$$,

so x divides y, as desired.

That’s a good proof!  Even though we had 2 existential generalizations and 1 universal generalization to do, and a lot of things to prove, the result came out very concise because we focused on the things that are central to the proof - letting and picking variables and showing that the values we chose satisfy the definition.

In trying to find your own proofs, the strategy above is something that you can do on any problem - first break down the definitions and figure out what sort of variables the definitions are forcing you to adopt, and then try to fill in that proof skeleton.  Often, just spelling out the definitions this way will make it obvious what you have to do to prove it!

So, we’ve seen that to prove a theorem we get to pick a variable whenever we see an existential quantifier, and we have to let an arbitrary variable whenever we see a universal quantifier.  What about when we want to use a theorem?  Let’s use theorem 1 to prove that:

_Thm 2_ Every natural number has at least one natural-number factor.

(Yes, this is a very easy theorem, and we don’t need theorem 1 to show this.  Bear with me).  If we recall that x is a factor of y if and only if x divides y, and we try to write out a proof skeleton, we get something that looks like:

Let $$n \in \mathbb{N}$$. Pick $$f = \_\_\_\_$$, and $$f \in \mathbb{N}$$ because \_\_\_\_.

{ Show that f divides n. }

Hence, f is a factor of n, so we are done.

Now we think that we might want to use theorem 1 to find f.  Well, when you want to use a theorem, the rule is opposite - wherever the theorem says “for all”, you can choose any value you want, but when the theorem says “there exists”, you have to just take whatever value it gives you.  Let’s do this to finish our proof:

_Proof_
Let $$ n \in \mathbb{N} $$.
By thm 1, there is some $$ x \in \mathbb{N}$$ such that, for all $$y \in \mathbb{N}$$, x divides y.
Pick $$f = x$$, and $$f \in \mathbb{N}$$ because $$x \in \mathbb{N}$$.
Pick $$y = n$$.

Then x divides y by the theorem, so f divides n. Hence, f is a factor of n, so we are done.

In practice, this is still too verbose, so when we get something from a theorem and then use it immediately, we will collapse the picks and lets.  This is how we might write a final version of our proof:

_Proof_
Let $$ n \in \mathbb{N}$$.  By theorem 1, there is some $$x \in \mathbb{N}$$ which divides every other natural number y, so in particular, x divides n.  Because x divides n, x is a factor of n, and we are done.

Nice!  You can use this trick with almost any problem to get an idea of what variables you need to use.  Just remember that to prove a theorem, for every “$$\exists$$” you get to pick a variable, and for every “$$\forall$$” you have to let an arbitrary variable, but when you are using a theorem it is the other way around.

### Leaving things out

Paragraph style informal proofs are focused on getting an idea across rigorously, but also concisely.  The idea is that there should be enough information in the proof that anyone can take it and turn it into a formal proof if they want to, but to leave out the technical details that aren’t relevant to the main idea of the proof.

Look back at our proof of theorem 1.  We already mentioned that we left out the generalization steps at the end.  We also glossed over the things that weren’t central to the idea of the proof, and would have merely clouded up the central ideas - these things include not only the existential and universal generalizations at the end, which were already implied by the way that we chose our variables at the start, but also the justifications that $$ 1 \in \mathbb{N}$$ and that $$1*y=y$$, which are not interesting in the context of the theorem, and the boolean logic that we would have to do to show that this actually satisfies the definition.

At this stage of the class, you can and should omit justifications for arithmetic and algebra, set theoretic identities, and boolean manipulations, unless that is specifically the subject matter of the problem.  Of course, you should only omit justifications if you can see at a glance what they would be - omissions are not a way of ignoring things that you do not understand, but a way of sparing the reader an obvious and tedious subproof.

For example, on the midterm and problem set 3, many people wrote things like “If AB then A by the definition of intersection.”  That is not what the definition of intersection says.  What is happening when you write that is that you come up with an identity that is obviously true, but that hasn’t been presented in the course.  This is fine; you don’t have to spell all of these things out if they are not central to the proof and they are simple enough that anyone in this class would see immediately that they are true.  However, then you aren't going to be able to come up with a valid justification, so just don't put one.

Another thing that you should omit from your proofs are anything that is actually unnecessary to the proof.  If there is something that you observe but do not use, take it out.  (When you are doing an exposition, trying to explain the topic to someone, these can be very important, but for the proofs we will ask you to do it’s better to be concise.  If you end up stating something that is not only unnecessary but also wrong, you will lose points).

There is a lot of stuff then that you can leave out of a proof, and they tend to be the small details and technicalities that most people would never consider.  (One of the reasons that we covered formal proofs and formal logic is so that you would know that those were there, hiding underneath.)  So what should you include?  There are few hard and fast rules, but some good rules of thumb would be to include all of the conditions and antecedents for definitions that you use explicitly, any theorems from class or the book that you invoke, the definitions of every variable you use, and everything that you personally found interesting or surprising.

There is also one rule that is universal: if there are a set of assumptions or conditions mentioned in the problem, you can bet that you will need to use them.  Your proof should be explicit when you invoke them (say "by assumption (i)," or something similar); if you don't use one of the hypotheses, this is definitely noteworthy and you should point out explicitly why it was not necessary.  It is sometimes unclear what you need to spell out and what you can elide, but the conditions / hypotheses / antecedents mentioned in the problem statement should never be left out!

### More Tips

Don't repeat yourself!  If you make some claim, and then decide that it requires justification, go back and replace that claim with the reasoning - if you leave it there, even with logic to back it up later, it will look to the reader like you are just pulling something totally out of the blue.

Informal definitions and ideas like the "arrow" description of functions are appropriate for motivations and for explaining the ideas to a casual observer, but not for proofs.  In a paragraph proof, you should not say, "because f is onto, everything has an arrow pointing into it," but instead, "because $$f$$ is onto, for every element $$y$$ in its codomain, there is an $$x$$ in its domain such that $$f(x)=y$$".  Proofs are for more than just getting the idea across - your proof should force the reader to admit that you are right, even if they believe otherwise.  To this end you must be precise.

Negations in the problem statement can make it more confusing whether you should pick or let a variable, because of DeMorgan’s laws for quantifiers.  Since $$\neg \exists x \, s.t. P(x)$$ is the same as $$\forall x \, \neg P(x)$$, should you pick a specific x, or let x be arbitrary?  Fortunately, there is a hard and fast rule for this one - always move the negation all the way into the problem statement before picking or letting.  In the example, this means that you would let x, because there is a “for all” in the front when the negation has been moved in.
