Proposer: Allison Kaptur
Year: 2014
Outcome: Accepted
Video: http://pyvideo.org/video/2567/import-ant-decisions

PyCon Talk Proposal
===================

Title: Import-ant Decisions
Duration: 30 min
Level: Novice
Categories: core

Description
===========
Suppose `import` didn't exist, and we had to invent it from scratch.  We'll look at the problem - code sharing and reuse across modules - and different ways we could solve it.  We'll come up with `import` from parallel universes and then reinvent python's actual implementation.  Finally, we'll import - using python's design - without using the `import` keyword.

Objectives
============
This talk will frame the way `import` works from the perspective of a language designer.  Everyone at PyCon probably already know what `import` does.  I will invite participants to think about the problem `import` solves, and other ways it could have been solved.  By the end, participants will understand why namespaces are one honking great idea.  They will also appreciate some of the tradeoffs python's `import` system makes and what parts of the problem it fails to solve.

Detailed Abstract
=================
### Part 1: What's the problem?
We'll start by looking at the problem that `import` tries to solve: allowing python code in one module access to code in another module.  I'll introduce the idea of a namespace and draw an equivalence between a namespace and a dictionary as an example of a simple lookup table.

### Part 2: How do we solve it?
We'll examine possible ways to allow code from one module to access code in another module.  I'll suggest different strategies and discuss their advantages and disadvantages. I'll show that those design decisions were made in other languages.  For example, we could insert all the code from Module B into Module A at compile-time.  The strengths: easy for the programmer to reason about (and others). The weaknesses: namespace collision and a rigid import system that can't change at runtime (and others).  The analogy: C's `#include`.  There will be three of these sections, each taking about three minutes to discuss.

### Part 3: Home to python
This section will transition from the last by proposing a solution to code-sharing that mirrors python's actual implementation of `import`.  We'll discuss strengths and weaknesses of the model.  I will briefly cover the steps of the import process: finding a module and returning a loader; checking in sys.modules for the module object; initializing a new module object, if needed; and executing the module's code object in the context of the new module object just created, if needed.

### Part 4: An artisan hand-crafted module
I will follow the steps required to create a module by hand, without using `import`.

Outline
=======
#####What's the problem? (5 minutes):
  - Consider the problem that `import` solves: We want to share and reuse-code across modules.
  - Introduce the idea of a namespace.
#####Possible solutions (from other languages) (9 minutes):
   -  Suppose that `import` didn't exist, and we had to invent a system.  What are some design choices we could make? Re-invent examples from C, Ruby, and Javascript.
#####Python's solution (4 minutes):
  -  Consider the design choices that `import` actually uses.
#####Import without import (5 minutes)
  -  Roll a module by hand, without using `import`, to underscore that there's nothing magical about it.
#####Conclusion (2 minutes)
#####Q & A (5 minutes)



Additional Notes
================
This talk is intended to be considerably different from Brett Cannon's talk last year "How Import Works". My talk is intended to give the novice-to-intermediate programmer a more robust mental model of what `import` does to their code, what problem it sets out to solve, and what tradeoffs its implementation made.  I plan to spend almost no time discussing the implementation details of import (directing the curious to Brett's talk).

There will absolutely not be any live coding in the hand-crafted module section.  The code in question goes something like this (and I think it could reasonably fit on four slides):

Step 1: Make a module

    >>> import types
    >>> new_mod = types.ModuleType('new_mod')
    >>> new_mod.__dict__
    {'__name__': 'new_mod', '__doc__': None}

Step 2: Check sys.modules and add our new module

    >>> import sys
    >>> "new_mod" in sys.modules
    False
    >>> sys.modules["new_mod"] = new_mod
    >>> "new_mod" in sys.modules
    True


Step 3: Execute some code in this context

    >>> new_mod.__dict__.keys()
    ['__name__', '__doc__']
    >>> exec "num = 50" in new_mod.__dict__
    >>> new_mod.__dict__.keys()
    ['__builtins__', '__name__', 'num', '__doc__']


Step 4: Ta-da

    >>> new_mod.num
    50

Rather than the trivial code executed in our new module above, I will compile a slightly more interesting code object and use that for the execution stage.

While this talk will discuss `import` equivalents in other languages (Javascript, Ruby, and C), no knowledge of these languages is necessary for participants.

Most of my prior speaking experience is giving 5-minute lightning talks at Hacker School, which I've done about 20 times over the last year. Unfortunately, none of these is recorded; I'll recreate one to record for you folks.

I blog about python at akaptur.github.io.

Additional Requirements
=======================
None
