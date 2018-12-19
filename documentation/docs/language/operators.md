## Operator precedence

Precedence rules in C2 are different from C/C++. Here are all precedence levels in C2, listed
from highest (1) to lowest (11):

1. `()`, `[]`, `.`, postfix `++` and `--`
2. prefix `-`, `~`, prefix `*`, `&`, prefix `++` and `--`
3. infix `*`, `/`, `%`
4. `<<`, `>>`
5. `^`, `|`, infix `&`
6. `+`, infix `-`
7. `==`, `!=`, `>=`, `<=`, `>`, `<`
8. `&&`, `||`
9. ternary `?:`
10. `=`, `*=`, `/=`, `%=`, `+=`, `-=`, `<<=`, `>>=`, `&=`, `^=`, `|=`
11. `,`

The main difference is that bitwise operations and shift has higher precedence than
addition and subtraction in C2. Bitwise operations also have higher precedence than
the relational operators. Also, there is no difference in precedence between && || or
between the bitwise operators.

### Examples

```c
a + b >> c + d

(a + b) >> (c + d) // C (+ - are evaluated before >>)
a + (b >> c) + d   // C2 (>> is evaluated before + -)


a & b == c

a & (b == c)       // C  (bitwise operators are evaluated after relational)
(a & b) == c       // C2 (bitwise operators are evaluated before relational)


a || b && c

a || (b && c)      // C  (&& binds tighter than ||)
(a || b) && c      // C2 (Same precedence, left-to-right evaluation)


a > b == c < d

(a > b) == (c < d) // C  (< > binds tighter than ==)
((a > b) == c) < d // C2 (Same precedence, left-to-right evaluation)


a | b ^ c & d

a | ((b ^ c) & d)  // C  (All bitwise operators have different precedence)
((a | b) ^ c) & d  // C2 (Same precedence, left-to-right evaluation)
```

The change in precedence of the bitwise operators corrects a long standing issue in the C
specification. The change in precedence for shift operations goes towards making the
precedence less surprising.

Conflating the precedence of || and &&, relational and equality operations, and all bitwise
operations was motivated by simplification: few remember the exact internal differences in
precedence between bitwise operators.

Left-to-right offers a very simple model to think about the internal order of operations,
and encourages use of explicit ordering, as best practice in C is to use parentheses anyway.

