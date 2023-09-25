
This section just shows some code fragments to give you a feel of the language.
As you'll see, most code *inside* function bodies is almost identical to C, but
code *outside* differs a bit.

### if-statement
```c
fn void demo_if(i32 a) {
    if (a > 0) {
        // ..
    } else {
        // ..
    }
}
```

### for-loop
```c
fn void demo_for() {
    // the for-loop is the same as C. You can declare the loop variable (i) inside.
    for (i32 i=0; i<10; i++) {
        io.printf("%d\n", i);
    }

    // also equal
    for (;;) {
        // ..
    }
}
```

### while-loop
```c
fn void demo_while() {
    // again exactly the same as C
    i32 a = 10;
    while (a > 0) {
        a--;
    }

    // not allowed in C, but Okay in C++ and C2
    while (Point* p = getPoint()) {
        // ..
    }
}
```

### enum + switch

```c

type Height enum u32 {
    Low = 0,
    Medium,
    High,
}

fn void demo_enum(Height h) {
    switch (h) {
    case Low:
        fallthrough;
    case Medium - High: // multi-condition case
        break;
    }

    // special checking of switching on enum types
    switch (h) {
        case Low - High:
            break;
        default:    // warning: default label in switch which covers all enumeration value
            break;
    }
}
```

The Switch statement differs from C in that the default case (if present) must be last. Also
empty switch statements are also not allowed.

Enum constants are always in their own namespace. This avoids global/module
namespace pollution and allows multiple enums to have the same constant name. But when switching on an
enum type, only the constant is allowed (for readability and consistency). When switching on a non-enum
type, the prefix must be used.

To prevent having to add artificial enum constants to determine the range of an enum (for int to enum
conversion etc.), C2 adds two keywords - _enum_min_ and _enum_max_.

```c
type State enum u32 {
    Start,
    Stop,
}

// enum_min/max() are compile time constant, just like sizeof() and elemsof()
const u32 lowest = enum_min(State);
const u32 highest = enum_max(State);
```

### struct types
```c
type Callback fn i32(char c);

type Status enum i32 {
    Idle,
    Busy,
    Done,
}

type MyData struct {
    const char* name;
    Callback open;
    Callback close;
    State status;

    // named sub-structs (x.other.value)
    struct other {
        i32 value;
        i32 status;   // ok, no name clash with other status
    }

    // anonymous sub-structs (x.value)
    struct {
        i32 value;
        i32 status;   // error, name clash with other status in MyData
    }

    // anonymous union (x.person)
    union {
        Person* person;
        Company* company;
    }

    // named sub-unions (x.either.this)
    union either {
        i32 this;
        bool  or;
        char* that;
    }
}
```

### struct initialization and incremental arrays
Incremental arrays are a special C2 feature. See [Variables](../language/variables.md)
section for more information.
```c
type Person struct @(packed) {
    const char* name;
    u8 age;
}

// the + indicates 'persons' is an incremental array
// the attribute indicates that the data should be placed into the indicated section
Person[+] persons @(section="__DATA, .mydata");

persons += { "John",  10 }
persons += { "Anna",  20 }
persons += { "Peter", 30 }

// you can add more entries later (in the same/other file of the same module)
persons += { "Alice", 40 }

fn void demo_elemsof() {
    i32 a = elemsof(persons);     // will be 4
}
```

### function pointers
```c
module demo;

type Callback fn i32(const char* text, i32 value);

// also shows function attribute
fn i32 my_callback(const char* text, i32 value) @(unused_params) {
    return 0;
}

Callback cb = demo.my_callback;

fn void demo_cb() {
    i32 result = cb("demo", 123);
    // ..
}
```

### inline ASM
C2 supports the GNU inline ASM syntax.
See [GNU site for syntax](https://gcc.gnu.org/onlinedocs/gcc/Extended-Asm.html).

```c
fn u64 rdtsc() @(inline) {
    u32 lo;
    u32 hi;
    asm volatile ("rdtsc" : "=a" (lo), "=d" (hi));
    u32 res = hi;
    res << 32;
    res |= lo;
    return res;
}
```

