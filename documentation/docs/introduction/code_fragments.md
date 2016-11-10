
This section just shows some code fragments to give you a feel for the language.
As you'll see, must code *inside* function bodies is almost identical to C, but
code *outside* differs just a bit.

### if-statement
```c
func void demo_if(int32 a) {
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
    // the foor-loop is the same as C. You can declare the loop variable (i) inside.
    for (int32 i=0; i<10; i++) {
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
    int32 a = 10;
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

type Height enum uint32 {
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
conversion etc), C2 adds two keywords _enum_min_ and _enum_max_.

```c
type State enum uint32 {
    Start,
    Stop,
}

// enum_min/max() are compile time constant, just like sizeof() and elemsof()
const uint32 lowest = enum_min(State);
const uint32 highest = enum_max(State);
```

### struct types
```c
type Callback func int32(char c);

type Status enum int32 {
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
        int32 value;
        int32 status;   // ok, no name clash with other status
    }

    // anonymous sub-structs (x.value)
    struct {
        int32 value;
        int32 status;   // error, name clash with other status in MyData
    }

    // anonymous union (x.person)
    union {
        Person* person;
        Company* company;
    }

    // named sub-unions (x.either.this)
    union either {
        int32 this;
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
    uint8 age;
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
    int32 a = elemsof(persons);     // will be 4
}
```

### function pointers
```c
module demo;

type Callback func int32(const char* text, int32 value);

// also shows function attribute
func int32 my_callback(const char* text, int32 value) @(unused_params) {
    return 0;
}

Callback cb = demo.my_callback;

func void demo_cb() {
    int32 result = cb("demo", 123);
    // ..
}
```
