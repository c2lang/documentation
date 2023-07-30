# Struct functions
__Struct functions__ are another new feature in C2.

Struct functions are _syntactic-sugar_ that makes code more readable.

The example below shows how it works:

```c
type Point struct {
    i32 x;
    i32 y;
}

fn void Point.add(Point* p, i32 x) {
    p.x = x;
}

fn void example() {
    Point p = { 1, 2 }

    // with struct-functions
    p.add(10);

    // without struct-functions (if defined as non struct-function)
    /// point_add(&p, 10);
}
```

## Rules
* Struct functions only work on struct/union types
* Struct functions are defined in the same module as the struct (not necessarily the same file!)
* For a type type named `Foo`, struct functions must start with `Foo.` prefix.
* There cannot be a struct member `x` and a struct function `Foo.x`.
* A (non-static) struct function is required to have 'Type\*' or 'const Type\*' as its first argument.
* A static struct function has no argument requirements and can only be called on the type itself,
    not an instance of the type. `Foo.add()` and `f.add()` cannot both exist.
* A struct function may also be assigned to variables of type Function with the correct prototype:
    _callback = Foo.add;_
* Sub-structs cannot have struct functions

Extra notes:

* It's possible to have public or non-public struct functions for a public struct.
* A struct-function itself is a regular function and can also be used as such
* It's also possible to use a struct function call if the variable of struct type is a member of
    another struct
```c
type Outer struct {
    Inner inner;
}

type Inner struct {
    // ...
}

fn void Inner.modify(Inner* inner, /* ... */) {
    // ...
}

fn void example() {
    Outer outer;
    outer.inner.modify(/* ... */);
    // translates to Inner.modify(&outer.inner);
}
```

* The following is OK, as struct functions implicitly dereference the object on which they are called:
```c
    Type* t = nil;
    t.init();
```

for more examples, see the tests in _c2compiler/test/Functions/struct_functions/_


### Complex example

See below a more complex example, which uses opaque pointers. The module `inner` offers an API:

```c
module inner;

import stdlib;
import stdio;

public type Shape struct {
    u8 sides;
    // ..
} @(opaque)

// a non-public struct-function
fn void Shape.init(Shape* shape, u8 sides) {
   shape.sides = sides;
}

// a static struct-function, called as Shape.create(..)
public fn Shape* Shape.create(u8 sides) {
    Shape* shape = stdlib.malloc(sizeof(Shape));
    shape.init(sides);
    return shape;
}

// a public, const struct-function, first argument: const Shape*
public fn void Shape.print(const Shape* shape) {
    stdio.printf("shape with %d sides\n", shape.sides);
}

// a public, struct-function, first argument: Shape*
public fn void Shape.free(Shape* shape) {
    stdlib.free(shape);
}
```

which is used by the module `outer`:

```c
module outer;

import inner local;

fn void example() {
    Shape* s = Shape.create(3);
    s.print();
    s.free();
}
```

_NOTE_: `outer` is not allowed to access Shape's regular members directly.

