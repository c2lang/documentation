
This section describes how the C2 compiler (c2c) is structured.

C2C is self-hosting, so written in C2 and compiling itself.

The major components are:

* AST - the AST object definitions, created by the parser/Builder.
* Parser - contains the Lexer, Parser and AST builder to create the AST tree.
* Analyser - analyser and marks the AST, generates diagnostics.
* Compiler - the top-level component that glues all components together.
* C_Generator - generates C code from the AST.
* IR_Generator - generates IR code from the AST.
* plugins/ - contains sources for the plugins
* libs/ - contains the interface files for the libraries (libc, SDL, lua, io_uring, curses, etc)


