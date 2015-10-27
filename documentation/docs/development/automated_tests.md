
The c2c sources include a *test/* directory will automated tests. To tool to run
these in located in <c2compiler/tools/tester>. These tests are modelled after
clang's unit test framework and are designed to create tests with minimal effort.

There are several types of tests:

* test generated diagnostic notes, warnings and errors
* test generated C code
* test generated IR code

