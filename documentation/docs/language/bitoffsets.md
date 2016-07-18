
A new feature in C2 are __bit-offsets__.

NOTE: dont confuse __bit-offsets__ with __bit-fields__, which are x-bit wide fields inside a
struct member.

Bit-offsets are used in code that often needs to fetch certain bits from registers, like
driver code.

The syntax of a bit-offset is `value[<highest bit>:<lowest bit>]`.

So the width will be highest - lowest + 1. The syntax was chosen to match
hardware specifications that often specify bits in registers in the same way.

```c
func void demo() {
    uint32 value = 0x1234;
    uint8 a = value[15:8]; // will be 0x12
    uint8 b = value[11:4]; // will be 0x23
    uint8 c = value[4:0]; // will be the lowest 5 bits

    // Both statements below are equal, the first C style, the second C2 style
    int32 counter = ((value >> 10) & 0x1F);
    int32 counter = value[14:10];
}
```

Overflows are checked and produce errors, like

```c

uint32 value1 = 0xffff;
uint8 a = value1[15:0];    // warning{implicit conversion loses integer precision 'uint16' to 'uint8'}

const uint32 Value2 = 0x1234;
int8 b = Value2[6:0] + 100;  // error{constant value 152 out-of-bounds for type 'int8', range [-128, 127]}

```
