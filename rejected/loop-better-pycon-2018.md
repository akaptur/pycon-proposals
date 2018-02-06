Loop better: a deeper look at iteration in Python
=================================================


Description
-----------

What's the difference between an iterable, an iterator, and a generator?  Do you know how to create infinitely long iterables?

Come to this talk to learn some of Python's slightly more advanced looping techniques.

Iterables are a very big deal in Python and they became even more important in Python 3.  There's quite a bit beyond the basics when it comes to loops and looping in Python.

In this talk, we'll take a step back and learn about how looping actually works in Python.  We'll then learn about a number of Python looping techniques that you've probably overlooked.

We'll learn about the difference between sequences, iterables, and iterators.  We'll also reveal the iterator protocol that powers for loops in Python.

After we learn the basics, we'll learn some techniques for working with infinite infinite iterables, generators, and generator expressions.

Attendees will walk away from this session with specific actionable recommendations for refactoring their own code as well as a wealth of new topics to look deeper into after the session.


Audience
--------

This talk is aimed at developers who are familiar with `for` loops, lists, and dictionaries.  Knowledge of generators, list comprehensions, or generator expressions would be useful, but none of those concepts are essential.  Audience members are not expected to know what an iterable, iterator, or sequence are.

Audience members will walk out of this talk understanding how all forms of iteration work in Python and how to avoid bugs due to the occasionally quirky way that Python handles iteration.


Outline
-------

Looping Problems (3 minutes)

- Introduction 
- Looping Twice
- Containment checking
- Unpacking

Review (4 minutes)

- C-style for loop
- Python's for loop
- Sequences (lists, strings, tuples)
- Other iterables (sets, dictionaries, generators)

Looping manually (2 minutes)

- Looping with indexes only works on sequences
- Converting to a list doesn't work for infinitely long iterables

Iterators (3 minutes)

- We can ask any iterable for an iterator
- Iterators give us the next item or a StopIteration exception
- Iterators are very single-purpose (like PEZ dispensers)
- Implementing a for loop using just a while loop and iterators

The Iterator Protocol (2 minutes)

- Iterators are single-use
- for loops use it
- tuple unpacking uses it
- star expressions use it
- many built-in functions use it
- generators are iterators

Even deeper (3 minutes)

- Iterators are also iterables
- Iterators are their own iterator
- Iterators allow lazy looping (generating items as you go)
- Generators are iterators
- The power of generators and infinite iterables

How to use this knowledge (6 minutes)

- Iterators enable laziness
- Iterators are everywhere
- You can make your own iterators (via generators)
- Iterators allow for more descriptive code

Revisiting Looping Problems (2 minutes)

- Exhausting iterators
- Partially consuming iterators
- Unpacking is the same thing as looping

Conclusion (1 minute)

- Iterables give iterators and iterators give each item individually
- Iterators are the backbone of looping in Python
- Iterable doesn't imply indexes.  Sequences are iterables but iterables may not be sequences.
- When someone says "iterable" they mean "something we can get an iterator from"


Notes
-----

I've given variations of this talk at PyCon AU 2017, PyGotham 2017, and North Bay Python 2017. I've also held webinars on this topic and I regularly teach these concepts via Intermediate Python training sessions for my on-site corporate training clients.

- PyCon AU Video: https://www.youtube.com/watch?v=JYuE8ZiDPl4
- PyGotham Video: https://www.youtube.com/watch?v=Wd7vcuiMhxU
- North Bay Python Video: https://www.youtube.com/watch?v=V2PkkMS2Ack
- Slides: http://treyhunner.com/loop-better
