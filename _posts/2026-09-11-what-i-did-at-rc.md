---
layout: post
title: "What I did at RC"
tags: RC projects
---

I've spent the summer at [Recurse Center](https://www.recurse.com/), a programming retreat in Brooklyn, at the recommendation of my friend Cory. These are some things I did there.

#### Participated in study groups

RC has regular study groups that anyone can organize and schedule. For example, people would get together every Friday to work through old Advent of Code challenges together. I did Agentic Adventures, Practical Deep Learning, Math Monday, and a short series investigating an open source board game AI.

#### Agentic Adventures

Agentic Adventures is a discussion group for people interested in modern LLMs and agents; we did different things every week. Some highlights:

- Amanda and I built a remote sandbox for a vibecoding agent that we could run with dangerously-skip-permissions.
- In the same vein, Michelle taught us how to set up a sandboxed environment for local agents using ollama and docker.
- This was what we built the ["Just One" game](https://thill.me/just-one-bot/) for, to investigate how agents can cooperate.
- We played around with minGPT together, building a small text predictor model trained on Romeo and Juliet to talk like Shakespeare.
- We did a basic LoRA to train a small model to talk more conversationally.

#### Practical Deep Learning

This was a study group working through the (first half of the) book [Practical Deep Learning](https://github.com/fastai/fastbook) by Jeremy Howard and Sylvain Gugger; we worked our way up through classical machine learning approaches, used or adapted prebuilt models, and then learned how to build up actual neural network models. The book gave a high level practitioner's introduction, which was good for building intuition and having a sense for how they work, as well as what can be built with them. Since I'm not interested in research myself, this was the level of understanding I wanted.

#### Math Monday

In a coffee chat, Sophia and I discovered we shared a passion for math and decided to start a discussion group for mathematical topics. This was meant to be a fun distraction and chance to learn and explore, so we would spend maybe the first half of each meeting on learning and then split into pairs to build things based on what we learned. Some fun things we worked on were:

- Working through [project euler](https://projecteuler.net/) problems together.
- Thinking our way through some problems from Peter Winkler's book [Mathematical Puzzles](https://math.dartmouth.edu/news-resources/electronic/puzzlebook/book/book.pdf).
- Drawing fractals and other generative art.
- Building programs using Voronoi diagrams, such as [this game](https://thill.me/voronoi-game/).
- Ben gave us an introduction to the Rocq proof assistant, and we worked our way through some boolean identities.
- We wrote a program to generate Hilbert curves for drawing with the pen plotter.
- Kenji and I built some fun extensions of the [chaos game](https://en.wikipedia.org/wiki/Chaos_game) in Desmos:

<img src="/assets/images/what-i-did-at-rc/desmos-chaos.jpg" alt="Some photogenic fractals we built in Desmos." width="700">


#### Reverse engineering a board game AI

Tony started a short sequence to dig into the code of the [Keldon](http://keldon.net/rftg/) AI for playing the board game [Race for the Galaxy](https://en.wikipedia.org/wiki/Race_for_the_Galaxy). We spent the first session just learning the rules and playing the AI, then dug into the code - an old school two-layer neural network with curated features. Claude was pretty good at interpreting the weights of the neural network and explaining what it reacted to; we spent some time trying to understand what the Keldon AI had learned about strategy. Takeaway was that econ strategies are much stronger than military in the base game (coinciding with player sentiment). One of the things that stood out to me was how many of the strongest weighted nodes corresponded to the presence of individual cards.

#### Finished up dodo

I wrapped up the mini language, [dodo](https://thill.me/2026/06/02/dodo.html), that I had made as part of my application to RC. This included what was probably the most interesting part, writing the tricky recursive code to implement the match statement. This was an interesting balance with LLMs - I didn't let them write any of the code for me, but it was nice to have someone writing up a nice spec and writing test cases and example programs. LLMs also made it easy to share what I did - try it out in a vibe-coded online REPL [here](https://thill.me/dodo/repl.html)!

#### Implemented DEFLATE

RC hosts a lot of talks by people discussing their projects. One I attended early in batch was by Josh on the ZIP file format, and his investigations about inconsistencies between implementations. It dove deep into details that I'd never thought about before. For example, if the unzipper that your virus scanner uses handles edge cases slightly differently than the unzipper that you use, it might see a file with malware in it as innocent, because it doesn't unzip the virus. This is a security vulnerability!

One part of the talk that caught my attention was the digression on the compression algorithm that zip uses, called DEFLATE. Kevan and I thought that reimplementing it sounded like a fun exercise, so we paired on it in a few sessions over the next few weeks.

DEFLATE is basically LZ77 (generalized run length encoding) + Huffman codes. It compresses text (or actually any sequence of bytes, but here we consider it as text) in two ways: (1) replacing repeated sequences with backreferences, e.g. "to be or not to be" could become "to be or not (13, 5)" where the part in parentheses means "go back 13 bytes, then copy 5 bytes from there", and (2) Huffman encoding, where we assign shorter sequences of bits to represent common bytes, and longer sequences for rare bytes - e.g., instead of 01100101 for 'e' and 00000111 for the bell character, you might use only 4 or 5 bits for the very common 'e', and 16 bits (or no representation) for the bell which never occurs in English text.

We wrote a [decompressor](https://github.com/kpthill/deflate-rust), working straight from the [spec](https://www.rfc-editor.org/rfc/rfc1951.txt), in Rust. It was my first time using the language but I found it to be quite nice. (Although I just gave up on understanding some of the data lifetime rules.) Most of the time, it just felt like what I wanted C to be. Still, it didn't eliminate any of the conceptual difficulties involved with bit packing - in debugging, we would frequently have to stop and write down the bit sequences we expected to compare them to what we got. No project made me feel more like a true hacker than that one.

#### Paired a lot

RC emphasizes the value of pairing - working on problems with other people. I'm a bit introverted even by RC standards, but I still did a bunch of that. Implementing DEFLATE with Kevan was the biggest example of pairing, but I did some other stuff as well. So with [Zaki](https://github.com/zmughal) and [Tommy](https://tommymaranges.com) we [used SAT solvers to solve sudoku](https://thill.me/2026/07/16/corn-puzzle-sat-solver.html) - bringing heavyweight tools to a relatively straightforward problem. I implemented the game of life with Seyoung, and we learned how to use the shell cursor to have it redraw itself neatly with each generation (or not neatly, if you get the command wrong). With William we implemented the game mastermind. With Bill we tried to make a new view for magit in emacs, which was honestly way too ambitious considering neither of us had written anything serious in emacs lisp before. But he did convince me to give doom emacs a try, and I've been enjoying it!

#### Vibecoded a bunch of mini projects

My goal coming into RC was to get the hang of them newfangled LLM things that all the kids are raving about. That's obviously part of the motivation for my participation in Agentic Adventures and Practical Deep Learning, but I also spent a bunch of time just vibecoding random things and trying to push the limits of what I could tackle.

One of them was a [rhythm game](https://thill.me/rhythm-game/) for the [rcade](https://rcade.dev/); this was the first one where I really leveraged LLM capabilities, since I had it make ten different prototypes and chose the one I liked the best.

<img src="/assets/images/what-i-did-at-rc/rhythm-game.png" alt="The opening of a song in the rhythm game." width="500">

Amanda and I experimented with how large of tasks we could leave an agent to do on its own; the result of that was a [fantasy map generator](https://thill.me/live-dangerously/) implementing climate modeling based on an academic [paper](https://doi.org/10.1007/s00371-020-01920-7).

<img src="/assets/images/what-i-did-at-rc/autobiomes-earth.png" alt="A pixellated map of the earth showing climate zones in different colors." width="700">

I also experimented with games that include agents as part of them. In one, we asked agents to play a word guessing game, Just One:

<img src="/assets/images/what-i-did-at-rc/just-one.png" alt="Several Claude Sonnet instances try to give hints to the word 'crocodile'." width="700">

The goal of the game is to give hints to a secret word without giving the same hint as another player; it was shocking how often all the agents playing would give the same hint, even at temperature 1. We resolved this by giving them all "personalities" which were really just topics; for example we'd be telling one agent to think about things in the context of sports, so their clue for "shell" might be "defense" or something, whereas the agent told to be a hippie might say "cancer" for the zodiac's crab.

In the same vein, I tried building a [diplomacy game](https://thill.me/llm-game/) where you can interact with agents as the main gameplay mechanic; unfortunately the main learning was that telling agents to "hold out on helping the player until they give you what you want" means that you get really stiff and combative characters. There's probably some secrets of character building and prompting that could have made this work, but I didn't find them.

Since both of these are browser games and I didn't want to spend my own tokens when people try them out, I also vibecoded a component that would let people use their own access keys or their own GPU (through WebLLM), which is what made those shareable!

At risk of falling into the classic yak shaving trap, I made an emacs mode for listing active vibecoding sessions:

<img src="/assets/images/what-i-did-at-rc/llm-supervisor.png" alt="A view of some of my current sessions." width="700">

Likewise, I had Claude build a bunch of improvements to this site and set things up so I could write this for you.

Later in the summer I got interested in trying to use these tools in a domain I was unfamiliar with, to answer questions I wasn't sure I could answer within my skill set. I was curious about how much overlap there could be between different languages' words, and vibecoded a tree search to find sequences of words in two languages which had the same (or very similar) pronunciations. This turns out to be very hard even for languages with similar phonologies (we used Indonesian and Farsi, based on their similar sound inventories according to PHOIBLE) but we did end up with a very mediocre poem, which you can see [here](https://thill.me/build-ten-thousand-things/poem.html).

Building on what I learned there, I then tried to vibecode a new constructed language. This was inspired by conlang critic's [video on Esperanto](https://www.youtube.com/watch?v=Sao9mCLy3Xo). Esperanto isn't as simple as you might prefer an international auxiliary language to be, and its vocabulary is notably eurocentric - which Zamenhof can hardly be blamed for, since he was making one of the first ones, but nevertheless I wondered if we could do better given all the information on the internet that we have available.

I probably owe a full post on this at some point (though if I start a new job, good luck on ever seeing that) but I'll give the nutshell version here: First, we used PHOIBLE data to put together a set of sounds that are common across languages, including recommended allophones to ensure that words remain producible and distinguishable for people with all sorts of native languages. Then we built a grammar piece by piece, loosely following a guide that real linguists use when describing the grammar of natural languages (the [Lingua Descriptive Studies questionnaire](https://www.eva.mpg.de/lingua/tools-at-lingboard/questionnaire/lingua-descriptive_description.php) by Bernard Comrie and Norval Smith). One cool trick there was that we made decisions based on the evidence of existing creole and (especially) pidgin languages described in [APiCS](https://apics-online.info/) (the Atlas of Pidgin and Creole Language Structures). Those languages were a good guide to what people who were used to different grammar systems actually converged on when they had to collaborate, so it was likely that many people around the world would find it intuitive or at least understandable. (Bonus fun fact - when you do this, you kind of naturally end up with a grammar that looks a lot like Chinese or Vietnamese - analytic languages using function words rather than declension to convey grammatical information.) Finally we built the lexicon from a collection of dictionaries for various world languages. Root words that came from an international canon - ones already borrowed into a bunch of languages - simply got borrowed and transliterated. (e.g. words like "phonology" or "coffee" became "fonologia" and "kafe".) For other words, we wrote an optimizer to select words that are both widely recognizable (favoring widely spoken languages like English or Chinese) and words that are representative of many different language families, to avoid Eurocentrism. Full disclosure - this part didn't really work right and I had Claude fake it just enough to get a sample script for my presentation. If I ever do make the full post, fixing that will be the first step.

At the end of the summer, I gave a talk about these projects, the slides for which are [here](https://thill.me/build-ten-thousand-things/). The slides were, of course, made by Claude.

#### Blogged a little?

I was more interested in doing than writing, but what little I did write you can see at the blog index. And I kept up an internally public series of checkins, which is what I'm referencing to write this post. If you ever do RC, I highly recommend following through on checkins, even if they're annoying at the time!

#### Played around

There's also just a lot of fun random tools and things to try at RC. For example, I made my first 3D print - a candle holder for a friend's 36th, and my 38th, birthdays:

<img src="/assets/images/what-i-did-at-rc/candle-trio.jpg" alt="Some views of the candle holder, showing how it shows different numbers when viewed from different sides." width="700">

The hub (RC's physical space in Brooklyn) also has an HP 7440A pen plotter that we like to mess around with. I drew a snapshot from a famous Go game and, with Jess, wrote a program to generate a Hilbert curve:

<img src="/assets/images/what-i-did-at-rc/plotter-pair.jpg" alt="A snapshot of the broken ladder game and a Hilbert curve, printed with the pen plotter." width="700">

We had weekly board game nights, and Dan and I led a few teaching sessions for the board game Go, as well as a trip to visit the Gowanus Go club.

#### Ooohed and aaaahed at other people's projects

Everyone at RC has their own cool projects going and part of the joy of it is getting to see what they're working on. I want to keep this section short, so I'm just going to list a fraction of the projects which have cool pictures attached:

Teresa made a photo studio setup that would give your picture a cool aura based on biometrics like your skin temperature:

<img src="/assets/images/what-i-did-at-rc/aura.jpg" alt="This unc got aura." width="400">

There were a bunch of cool drawings to see that people had made with the pen plotter:

<img src="/assets/images/what-i-did-at-rc/plot-wall.jpg" alt="A picture of the pen plotter wall." width="700">

Jagi made an awesome active decoration that would react and change its pattern based on the noise around it.

<img src="/assets/images/what-i-did-at-rc/jagi-decoration.gif" alt="Jagi's LED wheel." width="360">

Sarah and others made a "nintonio game mother" from an old e-ink screen:

<img src="/assets/images/what-i-did-at-rc/game-mother.jpg" alt="The nintonio game mother, playing pokemon." width="400">

Tony made an [exquisite corpse](https://en.wikipedia.org/wiki/Exquisite_corpse) game using the receipt printer.

<video src="/assets/images/what-i-did-at-rc/exquisite-corpse.mp4" width="340" autoplay loop muted playsinline style="display:block;margin:1.5em auto"></video>

You could play games other Recursers had built on the RCade cabinet:

<img src="/assets/images/what-i-did-at-rc/rcade-cabinet.jpg" alt="The RCade." width="400">

#### Wrote a "Return Statement"

RC alumni are encouraged to write a "return statement" - a brief reflection on their time at RC. That's this post!

If you were intrigued by it, consider [applying to a future batch](https://www.recurse.com/apply)!
