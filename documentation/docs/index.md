
### philosophy
*"The C programming language has been around for a long time and is still used
a lot nowadays. Its foundations are very solid, but other aspects
are showing their age. C2 attempts to modernize these parts, while keeping
the feel of C. It should be seen as an __evolution__ of C."*


### design goals
This philosophy leads to the following design goals:

* __higher__ development speed
* same/better speed of execution
* better compilation times
* integrated build system
* stricter syntax (easier for tooling)
* great tooling (formatting tool, graphical refactoring tool)
* easy integration with C libraries
* should be easy to learn for C programmers
* should help to avoid common mistakes

As such, since C2 is an __evolution__ of C, it has explicit __non goals__:

* higher-level features (like object-orientation, garbage collection, etc.)
* to be a completely new language

### advantages
So *why* would you choose C2 over C?

* __faster__ development.
* easy access to features like LTO (link-time optimization)
* better diagnostics (which, again, speeds up development)
* easier control over *architecture* with __c2reto__


### domain
New programming languages appear quite frequently nowadays. You may have
heard of some of the more notable projects like __D__, __Rust__, __Go__ 
and __Swift__. Each of these try to solve problems in a specific *domain*.
For example, both __D__ and __Rust__ aim to be *system-level* programming 
languages that have *higher level abstractions* - superceding C++.

*C2 aims to bridge the gap between the comfortability of C++, D and Rust,
and the ubiquity of C.* Low-level programs like bootloaders, kernels, 
drivers and system-level tooling are what C2 was designed for. It aims
to *directly replace C* - no more, no less.

### programming language evolution

![languages](introduction/languages.svg)

In the recent history of programming language development, there has been a trend towards more 
high-level abstractions. Java and C# are a prime example of this - they sacrifice performance
for abstraction. In the systems programming domain, newer languages like D, Rust and C++ all
provide more abstractions in the hope of making systems programming more friendly.

However, abstractions tend to sacrifice simplicity for ease of use. They don't seem to fill the gap 
for ultra low-level programs like operating systems. C is so common in this field because of its
simplicity! C2 aims to be nicer than C, but still as simple.
