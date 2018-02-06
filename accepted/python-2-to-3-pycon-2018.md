Python 2 to 3: How to Upgrade and What Features to Start Using
==============================================================


Description
-----------

The end of life for Python 2 is 2020. Python 3 is the future and you'll need to consider both your upgrade plan and what steps you'll take after upgrading to start leveraging Python 3 features.

During this talk we'll briefly discuss how to start **the process of upgrading your code to Python 3**. We'll then dive into some of **the most useful Python 3 features** that you'll be able to start embracing once you drop Python 2 support.

A number of the most powerful Python 3 features are syntactic features that are **Python 3 only**. You won't get any experience using these features until you fully upgrade. These features are an incentive to drop Python 2 support in existing 2 and 3 compatible code. You can consider this talk as a teaser of Python 3 features that you may have never used.

After this talk I hope you'll be inspired to fully upgrade your code to Python 3.


Audience
--------

This talk is aimed at Python 2 programmers who know they need to upgrade to Python 3 but they see upgrading as a chore and not an exciting opportunity. The talk is for experienced Python 2 programmers who haven't yet written Python 3 exclusive code: Python experience is required.

Python 2 programmers who've never written code meant to run in Python 3 will have knowledge of the tools they need to start upgrading their code to Python 3. Python programmers who are maintaining code that is both 2 and 3 compatible will be inspired to drop Python 2 support so they can start writing more readable and maintainable Python 3 code.


Outline
-------

Upgrading: the approaches (5 minutes)

- Gradual: add Python 3 support and eventually drop Python 2
- Sudden: Upgrade code to Python 3 and drop Python 2 immediately
- Extreme: rewrite the entire code base in Python 3

Steps for upgrading (3 minutes)

- Tests: have a process for verifying all functionality in your code
- Use tools to help you upgrade (2to3, python-future, six)
- Manually fix problems that can't be fixed automatically
- Start running your code in Python 3 in production
- Drop Python 2 support (if you haven't already)

Using a tool to help you upgrade (3 minutes)

- 2to3 is a one-way upgrade tool: it's good for sudden upgrading but not for gradual upgrading
- six is good for gradual upgrading but it's not an automated tool: it's a helper library
- python-future allows you to upgrade your code to support 2 and 3 at once: it's like 2to3 without dropping 2 support

Manually upgrading (2 minutes)

- You can use many Python 3 features within Python 2.7 by using __future__ imports, python-future should add these for you automatically though
- Fixing byte string vs unicode usage and character encodings
- Fixing which standard library features are used (things moved around)

We're not done yet (1 minute)

- Upgrading opens up so many new doors for you to open
- Many features in Python 3 can be used in Python 2 (__future__, PyPI libraries, etc)
- Many features in Python 3 cannot be used in Python 2 (syntactic changes especially)
- Python 3 exclusive code can look and feel so much better than code than supports both 2 and 3 (which is sort of awkward in-between code)

Big Python 3 syntax wins (10 minutes)

- Using multiple \**kwargs, merge dictionaries, etc
- f-strings vs format: why it's better and how to upgrade
- Keyword-only arguments (this is super awkward in Python 2)
- type hinting!
- Lots of standard library things like functools.total_ordering and UserDict actually work


Notes
-----

I do on-site corporate training for Python and Django teams as my day job. I also teach Python in free webcasts every week and I mentor folks at my local Python study group on a weekly basis.

I spoke at 8 Python and Django conferences in 2016 and 2017. This talk is a new one and I am hoping it will be well-received because Python 2 is very close to on its way out and Python 3 is improving greatly in each new release.

Some talks I've given:

- Readability Counts: https://www.youtube.com/watch?v=knMg6G9_XCg
- Comprehensible Comprehensions: https://www.youtube.com/watch?v=5_cJIcgM7rw
- Loop better: a deeper look at iteration in Python: https://www.youtube.com/watch?v=V2PkkMS2Ack
