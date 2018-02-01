
This section just shows some code fragments to give you a feel of the language.
As you'll see, most code *inside* function bodies is almost identical to C, but
code *outside* differs a bit.

### if-statement
```c
func void demo_if(i32 a) {
    if (a > 0) {
        // ..
    } else {
        // ..
    }
}
```

### for-loop
```c
func void demo_for() {
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
func void demo_while() {
    // again exactly the same as C
    i32 a = 10;
    while (a > 0) {
        a--;
    }

    // not allowed in C, but Okey in C++ and C2
    while (Point* p = getPoint()) {
        // ..
    }
}
```

### enum + switch
```c

type Height enum u32 {
    LOW = 0,
    MEDIUM,
    HIGH,
}

func void demo_enum(Height h) {
    switch (h) {
    case LOW:       // fallthrough
    case MEDIUM:
        // ..
        break;
    case HIGH:
        // ..
        break;
    }

    // special checking of switching on enum types
    switch (h) {
        case LOW:
        case MEDIUM:
        case HIGH:
            break;
        default:    // warning: default label in switch which covers all enumeration value
            break;
    }
}
```

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
type Callback func i32(char c);

type Status enum i32 {
    IDLE,
    BUSY,
    DONE,
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
type Person struct {
    const char* name;
    u8 age;
} @(packed)

// the + indicates 'persons' is an incremental array
// the attribute indicates that the data should be placed into the indicated section
Person[+] persons @(section="__DATA, .mydata");

persons += { "John",  10 }
persons += { "Anna",  20 }
persons += { "Peter", 30 }

// you can add more entries later (in the same/other file of the same module)
persons += { "Alice", 40 }

func void demo_elemsof() {
    i32 a = elemsof(persons);     // will be 4
}
```

### function pointers
```c
module demo;

type Callback func i32(const char* text, i32 value);

// also shows function attribute
func i32 my_callback(const char* text, i32 value) @(unused_params) {
    return 0;
}

Callback cb = demo.my_callback;

func void demo_cb() {
    i32 result = cb("demo", 123);
    // ..
}
```
