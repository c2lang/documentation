
# Naming

Unlike many other languages, C2 enforces the way the elements of the language
are named. This is done to unify coding styles and make it easier to read the
code. Currently only the first character of a name is enforced as either Upper-cased
or lower-cased, depending on the type of element.

Elements that must start with a *lower* case character:

* module names
* functions
* variables
* function arguments
* local constants
* struct/union members (including sub-structs/unions)

Elements that must start with an *Upper* case character:

* user-defined types
* enum constants
* global constants

## Libraries

Since external (C) libraries may require different names, no name checks are
performed on external libraries (or `.c2i` files).

## Constants

Since C2 borrows some type syntax from C, it's not always clear whether a
declaration is a *constant* or a *variable*, especially with pointer types.
See below:

```c
Point p = { 3, 4 }          // variable
const Point P = { 5, 6 }    // constant

const i32 Max = 5;        // constant

char* cp1 = "foo";          // variable
const char* cp2 = "foo";    // variable, since pointer itself is not constant

const char* const Cp3 = "foo";     // constant, but not supported *yet*

const i32[] Numbers = { 1, 2, 3}  // arrays are constant if the element type is constant

```

Also it's currently up to the developer to use CamelCasing or UPPERCASING for constants.

Example:
```c
type Struct struct {
    char* a;
    const char* b;
    i32 c;
    const i32 d;

    struct inner {
        i32 x;
    }
}

type Enum enum u8 { Foo, Bar, Faa }

i32 a;
const i32 B = 1;

char* c;
const char* d;
const Struct* e;

char[] f = "abcd";
const char[] G = "efgh";
const Struct[] H = { { nil, nil, 1, 2 }, { nil, nil, 3, 4 } }

fn void test1(i32 arg1, const i32 arg2, const char* arg3) {
    const i32 local1 = 2;
    const char* local2 = "";

    i32[4] local3;
    const i32[] local4 = { 1, 2, 3, 4 }
}


```

## Maximum identifier length

All identifiers (module/variable/function/type names) are limited to a length of 31 characters.
This ensures that full names (`module_name.item`) fit inside 64 characters.

