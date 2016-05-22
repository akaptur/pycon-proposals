# Javascript is Awe-ful

##Audience

Programmers with a passing familiarity with JavaScript, or an interest to learn more about _the_ front-end development language. 

## Objectives

* Introduction to some of the 'bad parts' ('wats') of JavaScript * Summary some of the new ECMAScript 5/6 additions to the JavaScript language * Understanding some of the reasons behind JavaScript's devoutness to backwards compatibility * Brief introduction to Batavia - python bytecode in the browser * Be introduced to some of the more interesting 'wats' of languages other than JavaScript. 

## Duration

I prefer a 30 minute slot

## Description

JavaScript. The language developers love to hate. It is full of 'bugs', 'wats', 'why-would-you-do-thats', and other annoyances; and yet it is the most used language in the world. If you develop web applications, you will have to touch JavaScript, so learn how to manage this timorous beastie. 

## Abstract

JavaScript is the front-end development language - all modern web-browsers use it. Couple this with it's 'general purpose' nature, it has long been considered to be the 'lingua franca' of the Web. Now days, many languages can be compiled into JavaScript, and it has recently seen a resurgence in it's use server-side with NodeJS and other uses.

If it's so powerful, why do developers loathe it?

We will discuss some of its... eccentricities, as well as the power that JavaScript can give, and how you can responsibly wield it. Also, this talk will discuss how it's not just JavaScript that has some interesting language features, as many other languages have some interesting thorns.

If you're a seasoned JavaScript developer, or even if you just have a passing familiarity, come along to this light-hearted crash-course in JavaScript, how to use it's good parts, and how it's not the only language that has 'wats'. 

## Outline

* Introduction to JS Wats (3 minutes)
 - Global variable declaration, Duck Typing, Arrays vs Objects, Equality
* Hate of JS (1 minute)
 - quips from various pundits about their loathing of the language
* Brief history of JS and ECMAScript (3 minutes)
 - Diving into the semantics of a summary of JS in 30 words or less
 - standardisation, validation of the statement 'most popular language ever'

* Detailing the Footguns (3 minutes)
 - explanation of reasons behind gripes presented in 'Introduction to JS Wats'
 
* Bugs and Backwards Compatibility (3 minutes)
 - parseInt, Number, typeof, all numbers are floats, unicode 
 - deep dive into fun with arrays and empty strings (`++[[]][+[]]+[+[]]` is `10`). 
 - deep dive (including C code) into why `typeof null` is `object`
 - explaining  why bugs exist due to backwards compatibility issues

* Security (1 minutes)
 - XSS, `eval`
* Avoiding JS with Frameworks (1 minute)
 - briefest of summaries

* Avoiding JS with specifically created languages (1 minute)
 - e.g. coffeescript, typescript

* Avoiding JS with cross-compilers (2 minutes)
 - Specific focus on [Batavia](https://github.com/pybee/batavia) - python bytecode in the browser

 * Harnessing JS outside the Browser (1 minutes)
 - node.js, etc
 
* Server side JS Wats (1 minute)
 - V8 optimisation bugs, etc (more if they appear between now and conference)

* ECMAScript 5 & 6 (3 minutes)
 - new functions (isArray, trim, let, import, spread)
 - rolling adoption issues

* Non-standard standards (1 minute)
 - specifically: `console.log` has never been standardised

* Extending JS (1 minute)
 - specifically: polyfill


* Summarise JS is full of awe ("awe-ful") ( 1 minute)

* Quick Fire Other Wats (3 minutes)
 - including: java, haskell, php, ruby, perl, powershell
 - specific focus on python (nod to "Investigating Python Wats" from PyCon 2015) 

* Final Summary (1 minute)
  - technical limitations to tools
  - tools solve problems
  - the choice of a tool does not make you a better/worse person


-----------


### Feedback (Private, via email):

Can you make more explicit the relevance to a Python audience specifically? Maybe some of the wats that are shared between Python and JavaScript? Maybe a sentence about how because Python is so commonly paired with JavaScript in the web stack, this is important background knowledge?

### Feedback, own notes:

This is an objectively strong talk, but has been submitted multiple times in this format to various PyCons, both US and regional, and has never been accepted. It has been accepted at other conferences, however. 

The issue with this abstract is that it doesn't relate to Python. Given the exact same actual talk, but an abstract that was directed at, say 'JavaScript for Pythonistas', or other direct relevence to Python programmers, it may be a better fit.
