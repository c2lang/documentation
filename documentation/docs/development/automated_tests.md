
The c2c sources include a *test/* directory with automated tests. The tool to run
these is located in the repository under
[tools/tester](https://github.com/c2lang/c2c_native/tree/master/tools/tester).
These tests are modelled after clang's unit test framework
and are designed to allow creating tests with minimal effort.

There are several types of tests:

* generated diagnostic notes, warnings and errors
* generated C code
* generated IR code
