__Bit-offsets__ are a new feature in C2.

NOTE: Don't confuse __bit-offsets__ with __bit-fields__, which are fields x-bits wide inside a
struct for memory conservation.

Bit-offsets are used in code that often needs to fetch certain bits from registers, such as
driver code.

The syntax of a bit-offset is `value[<highest bit>:<lowest bit>]`.

So the width is `highest - lowest + 1`. This syntax was chosen to match
hardware specifications that often specify bits in registers in the same way.

```c
func void demo() {
    u32 value = 0x1234;
    u8 a = value[15:8]; // will be 0x12
    u8 b = value[11:4]; // will be 0x23
    u8 c = value[4:0]; // will be the lowest 5 bits

    // Both statements below are equal, the first C style, the second C2 style
    i32 counter = ((value >> 10) & 0x1F);
    i32 counter = value[14:10];
}
```

Type overflows are checked and produce errors, like

```c

u32 value1 = 0xffff;
u8 a = value1[15:0];    // warning{implicit conversion loses integer precision 'u16' to 'u8'}

const u32 Value2 = 0x1234;
i8 b = Value2[6:0] + 100;  // error{constant value 152 out-of-bounds for type 'i8', range [-128, 127]}

```
