
Because C2 is an _evolution_ of C, this page summarizes the
_changes_ made and the design philosophy behind it.

TODO this page is in progress


* no Header files
    consequence: import statement, and modules

* no forward declarations
    philosophy: types the same thing twice, reduces development speed
    consequence: needs multi-pass compiler

* member access always through '.', not sometimes '->'
    philosophy: remove clutter/reduce change effort

* Unified type definitions

