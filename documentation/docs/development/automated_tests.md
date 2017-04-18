
The c2c sources include a *test/* directory with automated tests. The tool to run
these is located in <c2compiler/tools/tester>. These tests are modelled after
clang's unit test framework and are designed allow creating tests with minimal effort.

There are several types of test for testing:

* generated diagnostic notes, warnings and errors
* generated C code
* generated IR code

