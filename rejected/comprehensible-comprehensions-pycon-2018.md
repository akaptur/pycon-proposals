Comprehensible Comprehensions
=============================


Description
-----------

Finding list comprehensions incomprehensible?  Having trouble figuring out when to use list comprehensions or just plain `for` loops?

Come to this talk and learn the how, when, and why of list comprehensions.

We're going to learn:

- why list comprehensions exist and what the often overlooked benefit of using them is
- how to use list, set, an dictionary comprehensions as well as generator expressions
- when and how to easily turn a `for` loop into a comprehension
- why list comprehensions are often hard to read
- how to make your comprehensions even more readable than your loops
- when and how not to use comprehensions

You'll come away from this talk with a cheat sheet for helping you remember when and how to use comprehensions.


Audience
--------

This talk is meant for Pythonistas who finds list comprehensions hard to write.  Beginners to Python often find list comprehensions incomprehensible so this talk is largely targeted to them.

Attendees are expected to know how `for` loops in Python work and how lists and dictionaries work.  By the end of this talk, attendees will understand when and how they should use comprehensions to make code more descriptive and more readable.


Outline
-------

What are comprehensions (5 minutes)

- Definitions and assumptions: iterables, comprehension, generator expression, etc.
- `for` loops are a generic construct: they're meant for looping over an iterable
- list comprehensions are a special purpose construct: turning an iterable into an iterable
- possibly mention that they're equivalent to map and filter (or leave for inevitable question during Q&A)

How to create a comprehension (5 minutes)

- Copy-pasting your way from a loop to a comprehension
- Identifying opportunities for transforming code from a loop to a comprehension
- The types: dictionary comprehension, set comprehension, generator expression

How to make comprehensions readable (5 minutes)

- Comprehensions are often written all on one line
- Comprehensions have 3 parts and breaking them into three lines can make them much more readable
- Comprehensions can be descriptive but only if the component parts are descriptive
- Creating functions for map/filter or variable instead of complex expression for iterable can be very helpful for readability (`for` loops or comprehensions)
- Examples of multi-line comprehensions compared to `for` loops (audience can judge readability)

When to use generator expressions (5 minutes)

- Review of when to use comprehensions and how to transform loops to comprehensions
- Generator expression: what is it
- Anytime a comprehension will be looped over immediately and then discarded use a generator expression instead
- Examples of when to use a generator expression
- Identifying opportunities for using generator expressions (sum, any/all, min/max)

Don't Overdo it (3 minutes)

- List comprehensions are for turning one list into another
- Don't use comprehensions except for making lists (or other structures)
    - No print statements in comprehensions
    - No executing functions with side effects in comprehensions
- Only for the specific (but common) use case of turning one list into another

Recap (3 minutes)

- readability tips
- comprehensions copy-paste cheat sheet
- sum generator expression cheat sheet
- any/all generator expression cheat sheet
- min/max generator expression cheat sheet


Notes
-----

I've given this talk at PyCon Australia 2017 and I was complemented by teachers who had never seen comprehensions explained in such a thorough way. I also gave this talk remotely for PythonDay Mexico 2017.

I teach all of these tips during my on-site Python and Django corporate training sessions I do through my company, Truthful Technology LLC.

- Slides: http://treyhunner.com/comprehensible-comprehensions/
- Talk: https://www.youtube.com/watch?v=5_cJIcgM7rw
