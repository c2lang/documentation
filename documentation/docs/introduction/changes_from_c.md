
Since C2 is an _evolution_ of C, this page is here to summarize the
_changes_ made and the design philosophy behind it.

*TODO this page is in progress*


### Language changes

* no Header files

    _consequence_: import statement and modules

* no forward declarations

    _philosophy_: types the same thing twice, reduces development speed

    _consequence_: requires a multi-pass compiler

* member access always through dot-operator ' . ' , not sometimes ' -> '

    _philosophy_: remove clutter/reduce change effort

* Unified type definitions

* All global variables are initialized by default

* [Standardized attribute syntax](../language/attributes)

* better external library control

* [Improved operator precedence](../language/operators)

* [No auto-fallthrough in switch cases](../language/switch_statement/#auto-fallthrough)

* [no arrays as function arguments](../language/functions.md)

### New Features
C2 also introduces some *NEW* features:

* [BitOffsets](../language/bitoffsets)

* [Struct-functions](../language/struct_functions.md)

* [Modules](../language/modules)

* [Internal build-system](../build_system/intro)

* [Incremental arrays](../language/variables/#incremental-arrays)

* [SSwitch statement](../language/switch_statement/#sswitch-statement)

* [Plugins](../language/plugins.md)

* More tooling integration like dependency and refs file generation


