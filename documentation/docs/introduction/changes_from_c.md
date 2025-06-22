
Since C2 is an _evolution_ of C, this page is here to summarize the
_changes_ made and the design philosophy behind it.


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

* removed do .. while, since it's not needed without macros and error prone


### New Features
While C2 introduces some *NEW* features, these dont break the C philosophy and are
mainly *Syntax cleanup*, *syntactic-sugar* or remove a C anti-pattern:

* [BitOffsets](../language/bitoffsets)

* [Type-functions](../language/type_functions.md)

* [Modules](../language/modules)

* [Internal build-system](../build_system/intro)

* [Incremental arrays](../language/variables/#incremental-arrays)

* [Switch statement on strings](../language/switch_statement/#string-switch-statement)

* [Auto-arguments](../language/attributes/#auto-arguments)

* [Multi-condition case statements](../language/switch_statement/#multi-condition-case-statements)

* [Sane printf format specifiers](../language/printf_specifiers)

* [Compiler Plugins](../language/plugins.md)

* [Raw strings](../language/raw_strings.md)

* [init calls](../language/init_calls.md)

* More tooling integration like dependency and refs file generation


