# Playing With Python Bytecode

## Description

Ever wondered what Python is actually doing when it executes your code? 
Want to learn to hand-craft artisanal Python bytecode? In this talk, we 
explain CPython's internal code representation, and we demonstrate 
techniques for modifying code objects for fun and profit.

## Abstract

In this talk, we explain CPython's internal code representation,
demonstrating techniques for modifying code objects. We break down the
attributes of Python's code type, showing how one might construct a function
or module "from scratch" without the help of the compiler. We then introduce
techniques for manipulating the bytecode of existing code objects, which
allows us express functions that can't be written directly in Python. We
discuss some of the challenges that must be addressed to safely alter
bytecode, and we conclude by demonstrating some of the toys we've built with
codetransformer, our bytecode-manipulation library.

## Outline

How Does Python Execute Anything?
---------------------------------
(3 minutes; running total=3)

In this section, we provide an introduction to the `code` type.  In particular,
we draw attention to it's `co_code` attribute, which contains the actual
instructions executed by the interpreter.

- Write a simple function in the REPL.
- Call it.
- What actually happened there?
- Show the `__code__` attribute.
- Show the `co_code`.
- What the heck does is this inscrutable bytestring?

Intro to Bytecode
-----------------
(7 minutes; running total=10)

In this section we walk through a sequence of simple Python functions, showing
the disassembly of each and walking through the state of the CPython Virtual
machine as it executes the instructions.  The important ideas here are that
CPython is a stack-based virtual machine, that you can use the `dis` module to
inspect bytecode, and that there are many different kinds of instructions.
Important classes of instructions are `LOAD`s, `STORE`s, `JUMP`s, and `BINARY`
operators.

**First Example:**
(3 minutes)

Should teach the audience what the stack is, showing that values get pushed
onto and popped from the stack.  This section explains the basics of how to
read the output of the disassembly module.

Source:

    def add(a, b):
        return a + b

Disassembly:

    3           0 LOAD_FAST                0 (a)
                3 LOAD_FAST                1 (b)
                6 BINARY_ADD
                7 RETURN_VALUE

**Second Example:**
(1 minute)

Should teach the audience what a store looks like. This is intended to create
the simplest possible contrast with the previous example.

Source:

    def add_with_store(a, b):
        x = a + b
        return x

Disassembly:

    3           0 LOAD_FAST                0 (a)
                3 LOAD_FAST                1 (b)
                6 BINARY_ADD
                7 STORE_FAST               2 (x)

    4          10 LOAD_FAST                2 (x)
               13 RETURN_VALUE

**Third Example:**
(3 minutes)

Should teach the audience what a jump looks like, and introduces the
distinction between variables and constants.  This is foreshadowing two of the
important problems we'll be solving later: managing addition of new values to
the `co_consts`, and the ensuring correct resolution of jumps.

Source:

    def abs(x):
        if x > 0:
            return x
        else:
            return -x

Disassembly:

    3           0 LOAD_FAST                0 (x)
                3 LOAD_CONST               1 (0)
                6 COMPARE_OP               4 (>)
                9 POP_JUMP_IF_FALSE       16

    4          12 LOAD_FAST                0 (x)
               15 RETURN_VALUE

    6     >>   16 LOAD_FAST                0 (x)
               19 UNARY_NEGATIVE
               20 RETURN_VALUE
               21 LOAD_CONST               0 (None)
               24 RETURN_VALUE

Building a Code Object From Scratch
-----------------------------------
(10 minutes; running total=20)

In this section, we walk through the creation of a code object from scratch.
We give a short (~30-60 seconds each) treatment of each argument to `CodeType`,
culminating in the creation of full code object from scratch.

Python functions are objects just like anything else in Python.  This means
they must be instances of some type.  What would it take to build a function
object from "scratch"?

Let's make a simple function equivalent to:

    def addone(x):
        return x + 1

What do we need to pass to CodeType?

    In [1]: from types import CodeType

    In [2]: CodeType?
    Docstring:
    code(argcount, kwonlyargcount, nlocals, stacksize, flags, codestring,
          constants, names, varnames, filename, name, firstlineno,
          lnotab[, freevars[, cellvars]])

    Create a code object.  Not for the faint of heart.
    Type:      type

"Not for the faint of heart.  Great!"

The disassembly for our desired function:

    3           0 LOAD_FAST                0 (x)
                3 LOAD_CONST               1 (1)
                6 BINARY_ADD
                7 RETURN_VALUE

The bytecode:

    >>> addone.__code__.co_code
    b'|\x00\x00d\x01\x00\x17S'

This is easier to read interpreting the bytes as ints:

    >>> list(addone.__code__.co_code)
    [124, 0, 0, 100, 1, 0, 23, 83]

Each instruction is a single unsigned byte.  Some instructions accept
arguments, which modify the behavior of the instruction.  For example, `LOAD`
instructions take an argument describing where to find the value to be
loaded. For instructions that take arguments, the two bytes after the opcode
are used to describe the argument.

- `124, 0, 0`: `124` is the opcode for `LOAD_FAST`.  The next two bytes
  represent the argument to `LOAD_FAST`, which is an index into the fast (aka
  local) variable storage, interpreted as a little-endian 2-byte integer.  This
  says to load the local variable at index 0 onto the stack.

- `100, 0, 0`: `100` is the opcode for `LOAD_CONST`.  The argument to
  `LOAD_CONST` is also a 2-byte little-endian (note that the `1` comes first!)
  integer, representing the index into the constants tuple of the constant to
  load.  This says to load the constant at index 1 onto the stack.

- `23` is the opcode for `BINARY_ADD`.  `BINARY_ADD` is always the same, so
  there's no argument at the bytecode level.  This says to pop two values off
  the stack, add them, and push the result back onto the stack.

- `83` is the opcode for `RETURN_VALUE`.  Like `BINARY_ADD` it just operates on
  the stack, so it doesn't take a bytecode argument.

That covers the bytecode.  What other arguments do we need?

- `argcount` is the number of arguments accepted by the function. `1` for us.
- `kwonlyargcount` is the number of keyword-only arguments accepted by the
  function. `0` for us.
- `nlocals` is the number of local variables. `1` (`x`) for us.
- `stacksize` is the maximum number of values that will be on the stack at any
  given time.  In our case, the maximum value is 2: `x` and `1` are both on the
  stack right before we execute `BINARY_ADD`.
- `flags` is a 32-bit bitmask providing metadata about the function.  There are
  lots of flags:

        # Are we optimized?  Always True for functions.
        CO_OPTIMIZED = 0x0001

        # Do we create a new locals?  Always True for functions.
        CO_NEWLOCALS = 0x0002

        # Do we accept *args?
        CO_VARARGS = 0x0004

        # Do we acept **kwargs?
        CO_VARKEYWORDS = 0x0008

        # Is this function defined inside another function?
        CO_NESTED = 0x0010

        # Is this function a generator?
        CO_GENERATOR = 0x0020

        # The CO_NOFREE flag is set if there are no free or cell variables.
        # This information is redundant, but it allows a single flag test
        # to determine whether there is any extra work to be done when the
        # call frame is setup.
        CO_NOFREE = 0x0040

        # The CO_COROUTINE flag is set for coroutines created with the
        # types.coroutine decorator. This converts old-style coroutines into
        # python3.5 style coroutines.
        CO_COROUTINE = 0x0080
        CO_ITERABLE_COROUTINE = 0x0100

        # __future__ import flags.
        CO_FUTURE_DIVISION = 0x2000
        CO_FUTURE_ABSOLUTE_IMPORT = 0x4000  # Do absolute imports by default.
        CO_FUTURE_WITH_STATEMENT = 0x8000
        CO_FUTURE_PRINT_FUNCTION = 0x10000
        CO_FUTURE_UNICODE_LITERALS = 0x20000

        CO_FUTURE_BARRY_AS_BDFL = 0x40000  # Old April Fool's joke.
        CO_FUTURE_GENERATOR_STOP = 0x80000

    in our case, we want to set `CO_OPTIMIZED`, `CO_NEWLOCALS`, and
    `CO_NOFREE`, so `flags` should be `0x0001 | 0x0002 | 0x0040 = 0x0043 = 67`.
- `codestring` is the bytecode string we just built.
- `constants` is the constants tuple.  A standard function always has `None` as
  constant 0.  Our only other constant is the integer literal `1`, so the
  constants should be `(None, 1)`.
- `names` should be the names of global variables and names of attributes
  accessed.  We don't have any.
- `varnames` is a tuple of local variable names.  We should have `('x',)`.
- `filename` is whatever we want.  We'll use `'pycon2016.py'`.
- `name` is `'addone'`.
- `firstlineno` is whatever we want. We'll use `1`.
- `lnotab` is a binary format representing a mapping from instruction index to
  line number increment.
- `freevars` is a tuple containing names of variables that we're closing over.
  In this case it's just `()`.
- `cellvars` is a tuple containing names of variables that are closed over by
  functions defined inside of our function.  It's also just `()` here.

Putting them all together we have this:

    # This sadly doesn't accept keyword arguments.
    code = CodeType(
        1,                                     # argcount
        0,                                     # kwonlyargcount
        1,                                     # nlocals
        2,                                     # stacksize
        (0x0001 | 0x0002 | 0x0040),            # flags
        bytes([124, 0, 0, 100, 0, 0, 23, 83]), # codestring
        (1,),                                  # constants
        (),                                    # names
        ('x',),                                # varnames
        'pycon2016.py',                        # filename
        'addone',                              # name
        1,                                     # firstlineno
        bytes([0, 1]),                         # lnotab
        (),                                    # freevars
        (),                                    # cellvars
    )

Huzzah! Let's verify that this worked:

    >>> dis.dis(code)
      2           0 LOAD_FAST                0 (x)
                  3 LOAD_CONST               0 (1)
                  6 BINARY_ADD
                  7 RETURN_VALUE

Compare to code generated by Python.  Note that we're actually better because
we didn't add `None` unnecessarily.

Building a Function Object from Code
------------------------------------
(2 minutes, running total=22)

Great! Let's call our code object:

    Dismal Failure

Now that we've got a `code` object, how do we turn that into a proper function?

    In [1]: from types import FunctionType

    In [2]: FunctionType?
    Docstring:
    function(code, globals[, name[, argdefs[, closure]]])

    Create a function object from a code object and a dictionary.
    The optional name string overrides the name from the code object.
    The optional argdefs tuple specifies the default argument values.
    The optional closure tuple supplies the bindings for free variables.
    Type:      type

This is, thankfully, much simpler than `CodeType`.

    In [6]: addone = FunctionType(code, globals())

    In [7]: addone(2)
    Out[7]: 3

Modifying Existing Code Objects
-------------------------------
(3 minutes; running total=25)

It's not often that we want to construct a function (or module, or class) from
whole cloth. Python is already a pretty good language for generating bytecode.

What if we realized we actually wanted to add two instead of one?

    In [9]: addone.__code__.co_consts = (2,)
    ---------------------------------------------------------------------------
    AttributeError                            Traceback (most recent call last)
    <ipython-input-9-b34300c5889a> in <module>()
    ----> 1 addone.__code__.co_consts = (2,)

    AttributeError: readonly attribute

Thankfully, `code` objects are immutable, so we can't just mutate the consts in
place.  What we can do, however, is take an existing code object, copy all of
its attributes except the ones we want to replace, and create a new code object.

    def update_code(f, **kwargs):
        """Update attributes of a function's __code__."""

        code = f.__code__
        newcode = CodeType(
            kwargs.get('co_argcount', code.co_argcount),
            kwargs.get('co_kwonlyargcount', code.co_kwonlyargcount),
            kwargs.get('co_nlocals', code.co_nlocals),
            kwargs.get('co_stacksize', code.co_stacksize),
            kwargs.get('co_flags', code.co_flags),
            kwargs.get('co_code', code.co_code),
            kwargs.get('co_consts', code.co_consts),
            kwargs.get('co_names', code.co_names),
            kwargs.get('co_varnames', code.co_varnames),
            kwargs.get('co_filename', code.co_filename),
            kwargs.get('co_name', code.co_name),
            kwargs.get('co_firstlineno', code.co_firstlineno),
            kwargs.get('co_lnotab', code.co_lnotab),
            kwargs.get('co_freevars', code.co_freevars),
            kwargs.get('co_cellvars', code.co_cellvars),
        )
        return FunctionType(
            newcode, f.__globals__, f.__name__, f.__defaults__, f.__closure__,
        )

    In [1]: addtwo = update_code(addone, co_consts=(2,))

    In [2]: addtwo(3)
    Out[2]: 5

Updating the Bytecode
---------------------
(5 minutes; running total=30)

Updating the constants tuple is all well and good, but the real fun starts when
we update the **bytecode itself**.

    BINARY_ADD_OPCODE = 23
    BINARY_MULTIPLY_OPCODE = 20

    def add_to_mul(f):
        """
        Decorator that converts addition to multiplication.
        """
        instructions = list(f.__code__.co_code)
        new_instructions = []
        for instr in instructions:
            if instr == BINARY_ADD_OPCODE:
                new_instructions.append(BINARY_MULTIPLY_OPCODE)
            else:
                new_instructions.append(instr)
        return update_code(f, co_code=bytes(new_instructions))

Issues With Our Implementation
------------------------------
(2 minutes; running total=32)

    from string import ascii_lowercase

    def get_x(a, b, c, d, e, f, g, h, i, j, k, l, m, n, o, p, q, r, s, t, u, v, w, x, y, z):
        return x

    get_x(*ascii_lowercase)  # returns 'x'
    get_x_improved = add_to_mul(get_x)
    get_x_improved(*ascii_lowercase) # returns 'u'

    In [11]: dis.dis(get_x)
      2           0 LOAD_FAST               23 (x)
                  3 RETURN_VALUE

    In [12]: dis.dis(add_to_mul(get_x))
      2           0 LOAD_FAST               20 (u)
                  3 RETURN_VALUE

Other Challenges
----------------
(3 minutes; running total=35)

**Adding and Removing Instructions:**
(2 minutes)

The big challenge here is resolving jumps properly.

- If there's an absolute jump anywhere in the code, then it will break if I
  change the number of instructions before the jump target.

- If there's a relative jump, it will break if we change the instructions
  between the jump and its target.

Messing up jumps generally causes Python to segfault.  (Demo Example)

**Keeping Track of the Stack Size:**
(1 minute)

One of the pieces of data on the code object is "what's the maximum size of the
stack?".  Once you start manipulating the bytecode manually, it's very easy to
mess this up.

Messing up the stack size causes python to write to uninitialized memory.  This
is bad.

Demo of `codetransformer`
-------------------------
(5 minutes; running total=40)

In this section we go through a few examples of transformers that we've
written. The goal here is to provide a payoff for the audience having learned
all the material from the prior sections.  The hope is that by this point the
audience has all the knowledge they would need to write a proper library to do
bytecode manipulation themselves, so we can talk about some of the neat things
we've been able to do with such a library.

As we worked on these problems, we wanted a higher level abstraction for doing
terrible terrible hacks.  The result of that work is our `codetransformer`
library, which provides an API for writing your own transformers.

Core abstraction is to provide `Code`, `Instruction`, and `CodeTransformer`
classes for working with code without having to do low-level bit fiddling.

It's easy to convert to and from Python and `codetransformer` code objects.

(2 minutes)
**`ordereddict_literals`**

    from codetransformer.transformers.literals import ordereddict_literals

    @ordereddict_literals
    def ayy(a, b, c):
        return {'a': a, 'b': b, 'c': c}

    In [1]: ayy(1, 2, 3)
    Out[1]: OrderedDict([('a', 1), ('b', 2), ('c', 3)])

(2 minutes)
**`inline_compose`**

A common tool in functional programming is `compose`, which is a function that
takes multiple functions and pipes an input through all of them.

Example:

    def addone(a):
        return a + 1

    def timestwo(a):
        return a * 2

    In [1]: compose(addone, timestwo)(3)
    Out[1]: 7

    In [2]: compose(timestwo, addone)(3)
    Out[2]: 8

Under the hood, a standard implementation works by looping over each function,
calling it on the input, and passing the output on to the next function.

We can use `codetransformer.compose` to stitch together the functions directly
in bytecode.  This is substantially faster if you call the composed function in
a tight loop.

(Show Disassembly of `codetransformer.compose(addone, timestwo)`.)
(Show Slide of Timings)

**List of Other Transformers and Features**
(1 minute)

Transformers:
- `@implicit_self`
- `@islice_literals`
- `@pattern_matched_exceptions`
- `@asconstants`
- `@mutable_locals`

Other Features:
- Bytecode-to-AST Decompiler
- AST/Bytecode Pretty Printers
- Pattern Matching API for Bytecode Blocks
- Constant Deduplication
- Flag and Line Number Table Classes

Time for Questions
------------------
(5 minutes; running total=45)
