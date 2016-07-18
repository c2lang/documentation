
## naming

Unlike many other languages, C2 enforces the way way elements of the language
are named. This is done to unify coding styles and make it easier to read the
code. Currently only the first character of a name is enforced as either Upper
cased or lower cased, depending on the type of element.

Elements that must start with a *lower* cased character:

* module names
* functions
* variables

Elements that must start with an *Upper* cased character:

* user-defined types
* enum constants
* constants

### libraries

Since external (C) libraries may require different names, no name checks are
performed on external libraries (or .c2i files).

### constants

Since C2 uses some of the type syntax from C, it's not always clear whether a
declaration is a *constant* or a *variable*, especially with pointer types.
See below

```c
Point p = { 3, 4 }          // variable
const Point P = { 5, 6 }    // constant

const int32 Max = 5;        // constant

char* cp1 = "foo";          // variable
const char* cp2 = "foo";    // variable, since pointer itself is not constant

const char* const Cp3 = "foo";     // constant, but not supported *yet*

const int32[] Numbers = { 1, 2, 3}  // arrays are constant if the element type is constant

```

Also it's currently up to the developer to use CamelCasing or UPPERCASING for constants.
