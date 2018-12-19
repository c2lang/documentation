
### Philosophy
*"The C programming language has been around for a long time and is still used
a lot nowadays. Its foundations are very solid, but other aspects
are showing their age. C2 attempts to modernize these parts, while keeping
the feel of C. It should be seen as an __evolution__ of C."*


### Design goals
This philosophy leads to the following design goals:

* __Higher__ development speed
* Same/better speed of execution
* Better compilation times
* Integrated build system
* Stricter syntax (easier for tooling)
* Great tooling (formatting tool, graphical refactoring tool)
* Easy integration with C libraries
* Should be easy to learn for C programmers
* Should help to avoid common mistakes
* Should be easy to write a compiler for

As C2 is an __evolution__ of C, it also has explicit __non goals__:

* higher-level features (like object-orientation, garbage collection, etc.)
* to be a completely new language

### Advantages
So *why* would you choose C2 over C?

* __Faster__ development.
* Easy access to features like LTO (link-time optimization)
* Better diagnostics (which, again, speeds up development)
* Easier control over *architecture* with __c2reto__


### Domain
New programming languages appear quite frequently nowadays. You may have
heard of some of the more notable projects like __D__, __Rust__, __Go__ 
and __Swift__. Each of these try to solve problems in a specific *domain*.
For example, both __D__ and __Rust__ aim to be *system-level* programming 
languages that have *higher level abstractions* - superceding C++.

*C2 aims to bridge the gap between the comfortability of C++, D and Rust,
and the ubiquity of C.* Low-level programs like bootloaders, kernels, 
drivers and system-level tooling are what C2 was designed for. It aims
to *directly replace C* - no more, no less.
