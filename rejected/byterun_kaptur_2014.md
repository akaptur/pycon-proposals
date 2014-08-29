Proposer: Allison Kaptur
Year: 2014
Outcome: Rejected
Feedback:
The committee said, "Our basic concern with this proposal is that we thought it was a good proposal, but we weren't sure there was a sufficient audience for it."  Several months later, the committee reversed their decision and I was invited to give it after a similar talk had to back out of the lineup. Since I was already giving a talk that year, I decided not to give this one too.

===================
PyCon Talk Proposal
===================

Title: Bytecode: A Gateway Drug for Python Internals
Duration: 30 min
Level: Intermediate
Categories: core

Description
===========
Learn what python bytecode can (and can't) do for you!  We'll discuss how we can have compiled bytecode in an interpreted, dynamic language; look at the python virtual machine, and walk through a python bytecode interpreter written in python.

Objectives
==========
Working on byterun, Ned Batchelder's python bytecode interpreter written in python (link in notes), helped me understand why people can still talk about "compile-time" in python, when we all know python is a dynamic, interpreted language.  Participants in the talk will understand what bytecode is and what it isn't.  At the end, they'll feel ready to go read ceval.c just for the fun of it.

Detailed Abstract
=================
####Part 1: What bytecode is
A quick introduction to bytecode via the `dis` module.
####Part 2: What bytecode isn't
A view of how bytecode fits into interpreted python - and what bytecode alone doesn't include.
####Part 3: It's a stack machine!
We'll consider the python VM and walk through a simple example of pushing to and popping from it.
####Part 4: It's dynamic!
Understanding what bytecode doesn't include illuminates what it means to have a dynamic language.  We'll look at a sneaky example that you've probably written dozens of times.
####Part 5: Byterun
A bytecode interpreter can be - and has been - written in pure python.  We'll examine the main loop of Ned Batchelder's [byterun](https://github.com/nedbat/byterun) to gain a deeper understanding of the python bytecode interpreter.

Outline
=======
5 minutes: Bytecode & Dis.  A quick introduction to bytecode via the `dis` module.  `dis` is kind of readable, even for someone seeing it for the first time.  I'll give an example of two different python snippets that have the same bytecode (something like this):

    if foo:
        pass
    else:
        if bar:
            pass
        else:
            pass

    if foo:
        pass
    elif bar:
        pass

(I plan to cover much the same ground in this portion of the talk that I covered in [this blog post](http://akaptur.github.io/blog/2013/08/14/python-bytecode-fun-with-dis/); I have presented this as a five-minute talk at Hacker School in the past.)

5 minutes: What bytecode isn't.  When I first heard about python bytecode, I had the vague idea that everything in a python program is encapsulated in the bytecode.  This isn't close to true - and is central to having a dynamic language!  I'll spend one slide putting bytecode in context, showing that it's a small part of the python start-to-finish process.  (I will spend no time at all on any piece of the process not involving bytecode - this is not a everything-about-python talk.)

5 minutes: It's a stack machine!  I'll describe the python VM and spend a few slides illustrating a stack machine and showing opcodes pushing and popping to and from it.

5 minutes: Time to get dynamic.  I'll take an example of a character that can do two very different things (`%`) to underscore that the bytecode doesn't distinguish between using `%` to format a string and using `%` to take a remainder.

5 minutes: Byterun. I'll show some simplified code at the heart of byterun - specifically, the main loop that switches over opcodes.

5 minutes: Questions


Additional Notes
================
Byterun is Ned Batchelder's [project](github.com/nedbat/byterun), and I've been [collaborating with him](https://github.com/nedbat/byterun/pulls?direction=desc&page=1&sort=created&state=closed).

I attended Larry Hasting's talk on bytecode last year, and I acknowledge that I plan to cover similar ground. With all due respect, I thought his talk suffered slightly from its completeness: because he did such a thorough job of covering, e.g., all the attributes of the code object down to co_lnotab, the talk ended up a bit dry. My talk will spend less time on these aspects and more time showing how we can have compiled bytecode and a dynamic language - an apparent contradiction until you realize the limited scope of bytecode.

Most of my prior speaking experience is giving 5-minute lightning talks at Hacker School, which I've done about 20 times over the last year. Unfortunately, none of these is recorded; I'll recreate one to record for you folks.

I blog at akaptur.github.io.


Additional Requirements
=======================
None
