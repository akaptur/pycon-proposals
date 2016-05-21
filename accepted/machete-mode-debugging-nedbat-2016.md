# Title

Machete-mode debugging: Hacking your way out of a tight spot

# Category

Best practices

# Duration

30 min


# Description [If your talk is accepted this will be made public and printed in the program. Should be one paragraph, maximum 400 characters.]

When chasing mysterious bugs, it's helpful to use all the tools at your disposal.  We'll explore ways to use Python's dynamic tools to help track down the cause of head-scratching problems in large systems.  Tools include the inspect module, monkey-patching, trace functions, and the Python mechanisms at work behind them all. 


# Audience [Who is the intended audience for your talk? (Be specific; "Python programmers" is not a good answer to this question.)]

Python developers working in large mysterious systems, or who want to know more about the underpinnings of the Python features we use every day.


# Python level

Intermediate


# Objectives [What will attendees get out of your talk? When they leave the room, what will they know that they didn't know before?]

Attendees will: 1) learn some useful debugging techniques, 2) understand more of the machinery that makes Python do its thing, and 3) hear some interesting war stories.


# Detailed Abstract [Detailed description. Will be made public if your talk is accepted.]

Python provides a malleable and introspectable runtime environment.  Good discipline requires restraint from using too many of those dynamic tools, but sometimes they are invaluable.

When chasing mysterious bugs, it's helpful to use all the tools at your disposal.  In this talk, we'll investigate some ways to use Python's dynamic nature to help track down the cause of head-scratching problems in large systems.  We'll touch on the inspect module, monkey-patching, and trace functions, among others.  Along the way, we'll talk about many fundamental Python mechanisms: the ones that cause the problems, the one that diagnose them, and the ones that solve them.

Good engineering principles go out the window in our pursuit of the information we need!


# Outline

* Python's runtime is a giant collection of unprotected inter-related objects
    - This can cause problems
    - Let's use it to our advantage, to **find** the problems!

* When something goes wrong
    - Need to get information about what's going on
    - How can we use Python dynamic nature to extract clues?
    - Clean code doesn't matter, we just need the information

* Case 1: Double importing of modules
    - Problem: Django 1.8 complains if a model is imported twice.
        - Somehow my module is being imported twice! Where?
    - Modules are supposed to only run once
        - sys.modules keeps the module objects
        - how can this happen?
        - sys.path is used to find modules
        - overlapping entries in sys.path can cause trouble
    - How do modules work?
        - module-level code is executed when the module is imported
    - inspect.stack at module level to find where it's happening
        - inspect.stack shows the current call stack
    - Use it at module level to show who is importing the module

* Case 2: Finding temp file creators
    - (Based on: [Finding temp file creators](http://nedbatchelder.com/blog/201503/finding_temp_file_creators.html))
    - Problem: Too many temp files are being made with `tempfile` and not deleted.
        - Who is making them?
    - Grepping 200k lines of code is infeasible
    - inspect.stack again!
        - But where to put the call?
    - Monkeypatching
        - Modules are mutable objects
        - You can import a module then change it.
        - Even the standard library, it isn't special
    - Can be tricky to get the patch right
        - `from tempfile import mkdtemp`
        - Too late to monkeypatch
    - Read the tempfile.py code
        - Helper function `_get_candidate_names`
    - How to run the monkeypatch code as early as possible
        - Put the code in firstthing.py
        - Add `import firstthing` to aaa.pth
    - read the stdlib source to find a good pinch point
        - open source!
    - Change `_get_candidate_names` to put a compact stack trace into the name of the temp file!

* Case 3: Who is changing sys.path?
    - (Based on: [Ad-hoc data breakpoints](http://nedbatchelder.com/blog/201311/adhoc_data_breakpoints.html))
    - Problem: sys.path is getting changed incorrectly, and we don't know who is doing it.
    - Use trace function to break into the debugger when sys.path is modified.

* Case 4: Why is random() different?
    - (Based on: [Hunting a random() bug](http://nedbatchelder.com/blog/201302/hunting_a_random_bug.html))
    - Running code with a seeded random(), why is it different the second time it's run?
    - "Different second time" sounds like something happening during import
        - Remember: modules execute on first import, not on second import
        - "import sympy" seems interesting
        - sympy is consuming a random number from the sequence 
    - Monkey-patch random.random with a bomb:
        - random.random = lambda: 1/0
        - Then import sympy
    - Now the cause is apparent:
        - `def __init__(self, ..., seed=random.random()):`
    - This is in a test helper method, why is it even imported?
        - Putting imports in `__init__.py` makes it too easy to import everything all the time.
    - Bonus irony: the seed argument default was never used!

* Case 5: Where did that User come from?
    - (Based on: [Stack ninjas](http://nedbatchelder.com/blog/201108/stack_ninjas.html))
    - Problem: in a large test suite, a User is created and not deleted, polluting other tests
        - Which test made a User and didn't delete it?
    - inspect.stack to add a stack trace to User objects
        - Modifying Django code is OK, it's just temporary
    - But: User has no place to put the stack trace!
        - Round-tripped through database
        - Can't just add ad-hoc attributes
    - Keep a global dict associating User objects with stack traces
        - Can't use primary key, those are re-used.
        - Timestamps are unique enough.

* Conclusion:
    - Python's runtime is much more like Lisp than like C or Java.
    - This can cause problems, but it can also help you solve them.
    - Don't be afraid to get dirty to debug things.

# Additional notes [Anything else you'd like the program committee to know when making their selection: your past speaking experience, open source community experience, etc.]


# Additional requirements [Please let us know if you have any specific needs (A/V requirements, multiple microphones, a table, etc). Note for example that 'audio out' is not provided for your computer unless you tell us in advance.]
