
Since C2 is an _evolution_ of C, this page is here to summarize the
_changes_ made and the design philosophy behind it.

*TODO this page is in progress*


### Language changes

* no Header files
    consequence: import statement and modules

* no forward declarations
    philosophy: types the same thing twice, reduces development speed
    consequence: requires a multi-pass compiler

* member access always through '.', not sometimes '->'
    philosophy: remove clutter/reduce change effort

* Unified type definitions

* All global variables are initialized by default

* [Standardized attribute syntax](../language/attributes)

* better external library control

* [Improved operator precedence](../language/operators)

* [No auto-fallthrough in switch cases](../language/sswitch_statement.md)

* [no arrays as function arguments](../language/functions.md)

### New Features
C2 also introduces some *NEW* features:

* [BitOffsets](../language/bitoffsets)

* [Struct-functions](../language/struct_functions.md)

* [Modules](../language/modules)

* [Internal build-system](../build_system/intro)

* [Incremental arrays](../language/variables/#incremental-arrays)

* [SSwitch statement](../language/sswitch_statement.md)

* More tooling integration like dependency and refs file generation


