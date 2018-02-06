Easier Classes: Python Classes Without All The Cruft
====================================================


Description
-----------

When bundling up data, sometimes tuples and dictionaries don't quite cut it.  Python's classes are powerful tools for data storage and manipulation, but it can take quite a bit of boilerplate code to make a well-behaved Python class.  In this talk we're going to discuss how a well-behaved class should work and take a look at a number of helper libraries for creating well-behaved classes.

We'll first see how to make classes with proper string representations, comparability, iterability, and immutability.  Then we'll dive into helper tools built-in to the standard library and available in third-party libraries.

We'll look at namedtuple, NamedTuple (not a typo), attrs, and the new Python 3.7 dataclasses.

Most of the libraries discussed in this talk are only available in Python 3, so if you're not using Python 3, hopefully this talk will encourage you to upgrade.


Audience
--------

This talk is aimed at Python developers who write a lot of small data-holding classes.  Audience members are expected to know how classes work in Python.  Knowledge of dunder methods and operator overloading would be helpful to understanding but is not essential to appreciating this talk.

Audience members will walk out of this talk inspired to remove many lines of repetitive boilerplate code from their classes by embracing one of the tools discussed during this talk.


Outline
-------

Intro (1 minute)

The Problem: Making Pythonic Classes (5 minutes)

- We always need a __init__
- We should always have a __repr__
- It's often useful to have a __eq__ (get __ne__ for free in Python 3)
- It frequently makes sense have implement __lt__, __ge__, __gt__, __le__
- functools.total_ordering in Python 3
- Hashability (for use in sets and dictionaries)
- Iterability (when unpacking makes sense)

Solution 1: collections.namedtuple (4 minutes)

- Built-in to the standard library since Python 2.6
- Convenient for simple classes without functions
- Can also be inherited from to add custom functionality
- The _fields attribute is useful for extending functionality
- There are quirks
    - errors in usage sometimes give really bizarre tracebacks
    - inheritance from tuple results in some oddities (e.g. multiplication and addition)
    - immutable (what if you want to change stuff?!)
    - no type hinting
    - Type of namedtuple is ignored

Solution 2: typing.NamedTuple (2 minutes)

- Built-in to the standard library since Python 3.5
- Like collections.namedtuple but a much nicer syntax
- Type hinting built straight in!
- Allows default values
- Still has most of the namedtuple quirks

Solution 3: attrs library (3 minutes)

- Pros
    - Works for both mutable and immutable things
    - More flexible than namedtuple or NamedTuple
    - Easy conversion to other types
    - Allows for validators, typing hinting syntax, etc
- Cons
    - Not in the standard library
    - Usage is a little odd (I find @attr.s awkward to explain)

Solution 4: dataclasses (4 minutes)

- Available as a third-party library
- Borrows many attrs features but keeps API very simple
- Will be bult-in to the standard library in Python 3.7
- Some things dataclasses doesn't support that attrs does

Data Classes Recipes (4 minutes)

- Immutability
- Orderability
- Iterability
- Hashability
- Inheritance
- Default values
- Extra attributes

Takeaways (2 minutes)

- You don't always need custom classes for your data, but it can be very handy
- Your commonly used classes should feel Pythonic
- You don't want lots of boilerplate in your classes
- Use dataclasses or attrs when you need custom data-heavy classes


Notes
-----

I do on-site corporate training for Python and Django teams as my day job. I also teach Python in free webcasts every week and I mentor folks at my local Python study group on a weekly basis.

I spoke at 8 Python and Django conferences in 2016 and 2017. This talk is a new one and I am hoping it will be well-received because Python 2 is very close to on its way out and Python 3 is improving greatly in each new release.

I teach my students about namedtuples and creating Pythonic classes quite frequently and I'm very excited about the possibilities that dataclasses opens up for Python teachers.

Here are three webinars I've given where I've discussed namedtuples and Pythonic classes:

- Tuples: https://www.crowdcast.io/e/tuples
- Classes in Python: https://www.crowdcast.io/e/classes
- Classes in Python: https://www.crowdcast.io/e/classes-2

Some conference talks I've given:

- Readability Counts: https://www.youtube.com/watch?v=knMg6G9_XCg
- Comprehensible Comprehensions: https://www.youtube.com/watch?v=5_cJIcgM7rw
- Loop better: a deeper look at iteration in Python: https://www.youtube.com/watch?v=V2PkkMS2Ack
