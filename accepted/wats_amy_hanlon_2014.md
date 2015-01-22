Proposer: Amy Hanlon
Year: 2014
Outcome: Pending

# Title
Investigating Python Wats

# Category
Python Core

# Duration
30 minutes

# Description
> If your talk is accepted this will be made public and printed in the program. Should be one paragraph, maximum 400 characters.

Many of us have experienced a "wat" in Python - some behavior that totally mystifies us. We'll look at three areas where wats arise - identity, mutability, and scope. For each of these three topics, we'll look at some common surprising behaviors, investigate the cause of the behaviors, and cover some practical tips on how to avoid related bugs.

# Audience
> Who is the intended audience for your talk? (Be specific; "Python programmers" is not a good answer to this question.)

Programmers who've experienced bugs around identity, mutability, or scope, but haven't developed a rigorous understanding of why the bugs occurred

# Python Level
Novice

# Objectives
> What will attendees get out of your talk? When they leave the room, what will they know that they didn't know before?

Attendees will leave the talk with a more rigorous understanding of identity, mutability, and scope, tools for avoiding bugs caused by common surprising behaviors, and some fun Python trivia.

# Detailed Abstract
> Detailed description. Will be made public if your talk is accepted.

## Identity
First, we'll look at some common CPython gotchas involving object identity. We'll try to guess the value of various `is` statements, and then see what the actual value is.

Then, we'll discuss why some of these statements evaluate to `True` and others `False`, and how object identity differs from equality. We'll discuss some implementation details of CPython, and how they lead to `is` evaluating differently for integers between -5 and 256 and integers outside that range.

Finally, we'll cover some quick practical tips about how to avoid bugs resulting from using the `is` statement.

## Mutability
Again, we'll look at some common Python gotchas, this time caused by mutability.

Then, we'll investigate which common Python objects are mutable and which aren't, which operations mutate Python objects, and what happens when you mutate an object. In the case of mutable default arguments, we'll even see how we can mutate the default argument from outside the function body using some internals that Python exposes to us.

We'll end this section by discussing some practical tips on how to avoid bugs due to mutability, how to make real copies, and a common alternative to using mutable default arguments.

## Scope

First we'll see some examples of Python name resolution, and we'll try to guess what object the name is bound to, if any, with some surprising results.

After seeing some examples, we'll investigate how the Python interpreter does name resolution, and we'll explore the meaning of the cryptic `UnboundLocalError`.

# Outline

## Introduction/overview of outline (1 min)

## Identity (7 min)

1. Trivia (3 min)

    I'll present some examples of using the `is` statement, allowing the audience to guess the value before revealing the (sometimes surprising) answer.

    Some examples:

        >>> "" is ""
        True
        >>> 0 is 0
        True
        >>> [] is []
        False
        >>> {} is {}
        False
        >>> () is ()
        True
        >>> a = 256
        >>> b = 256
        >>> a is b
        True
        >>> a = 257
        >>> b = 257
        >>> a is b
        False
        >>> a = 257; b = 257
        >>> a is b
        True

2. Why? (3 min)

    I'll explain in detail the example of using the `is` statement with `256` vs `257`, and how CPython stores an array of integers with the values of -5 to 256. I'll use a diagram to represent names referencing objects stored in memory, and show that with `257`, we actually store two separate instances of integers that happen to have the same value.

3. Tips (1 min)

    I'll provide some practical tips on when to use `is` vs `==`.

## Mutability (12 min)

### Part I: Intro to Mutability (5 min)
1. Trivia (1 min)

    I'll present some examples of surprising effects of mutability, while allowing the audience to first guess what will happen, like in the first section.

    An example:

        >>> favorite_things = ['cats', 'dragons']
        >>> copy_of_favorite_things = favorite_things
        >>> copy_of_favorite_things.append('rainbows')
        >>> favorite_things
        ['cats', 'dragons', 'rainbows']

2. Why? (2 min)

    I'll show a diagram of a list in memory, and then show what happens to that list with each line of code in the example. I'll show that a statement like `copy_of_favorite_things = favorite_things` binds the name `copy_of_favorite_things` to the same object that `favorite_things` is bound to, rather than creating a new object in memory.

3. Tips (2 min)

    I'll show the most common mutable objects in Python, and show a couple different ways to make real copies. I'll introduce the concept of shallow copies and deep copies.

### Part II: Mutable Default Arguments (7 min)

1. Trivia (1 min)

    I'll present the following example of using mutable default arguments, allowing the audience to first guess what happens when you call the function multiple times, and then revealing the answer.

        >>> def append_cat(l=[]):
        ...     l.append('cat')
        ...     return l
        ...
        >>> append_cat()
        ['cat']
        >>> append_cat()
        ['cat', 'cat']

2. Why? (3 min)

    I'll explain how the default value of `l` for the function is a list that is created once (when we define the function), by introducing the `__defaults__` attribute of function objects (`__defaults__` in Python 2.x). I'll show the value of `__defaults__` before executing the function, after executing it once, and after executing it a second time. Then I'll show that we can even mutate the value of `__defaults__` from outside the function body.

3. Tips (3 min)

    I'll show a common alternative for using mutable default arguments:

        >>> def append_cat(l=None):
        ...     if l is None:
        ...         l = []
        ...     l.append('cat')
        ...     return l

    I'll also show how, for fun (although not recommended in practice), you could take advantage of mutable default arguments to cache the results of an expensive algorithm:

        def sorting_hat(student, cache={}):
            if student in cache:
                return cache[student]
            else:
                house = expensive_alg(student)
                cache[student] = house
                return house

    And then I'll show how we could be malicious Hogwarts hackers by modifying the cache from outside the function body using `__defaults__` if we're displeased with how the `expensive_alg` sorts us.

## Scope (5 min)

1. Trivia (2 min)

    I'll cover a couple examples of name resolution, allowing the audience to guess whether a `NameError` will occur or not.

    Some examples:

        >>> def foo():
        ...     print a
        ...
        >>> a = 1
        >>> foo()
        1       # no NameError!

        >>> def foo():
        ...     a += 1
        ...     print a
        ...
        >>> a = 1
        >>> foo()
        Traceback (most recent call last):
          File "<stdin>", line 1, in <module>
          File "<stdin>", line 2, in foo
        UnboundLocalError: local variable 'a' referenced before assignment

2. Why? (3 min)

    I'll discuss how Python conducts name resolution (local scope, enclosing scope, global scope, builtins), and what this cryptic `UnboundLocalError` means.

## Q&A (5 min)

# Additional Notes
> Anything else you'd like the program committee to know when making their selection: your past speaking experience, open source community experience, etc.

I gave a version of this talk at PyGotham this year, and it was very well received -- the room was so packed that some attendees couldn't get in. It was recorded, and I'll post the link here when it's been uploaded to pyvideo.org. Here are the [slides](http://www.slideshare.net/AmyHanlon/python-wats-uncovering), and here are [some](https://twitter.com/ncoghlan_dev/status/503429783770779648) [tweets](https://twitter.com/paultag/status/503430094531346432) about the talk.

I've also given a talk on replacing a keyword in Python at Open Source Bridge and Hack and Tell NYC. Here is the [abstract](http://opensourcebridge.org/sessions/1206) and the [slides](http://www.slideshare.net/AmyHanlon/replacing-import-with-accio). I have some previous experience speaking as a tour guide at Natural Bridge Caverns in Texas.

I blog at http://mathamy.com/

# Additional Requirements