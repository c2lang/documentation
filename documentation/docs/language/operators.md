## Operator precedence

Precedence rules in C2 are different from C/C++:

1. `()` `[]` `.` postfix `++` and `--`
2. `-` `~` prefix `*` `&` prefix `++` and `--`
3. `*` `/` `%`
4. `<<` `>>`
5. `^` `|` `&`
6. `+` `-`
7. `==` `!=` `>=` `<=` `>` `<`
8. `&&` `||`
9. `?:` (ternary operator)
10. `=` `*=` `/=` `%=` `+=` `-=` `<<=` `>==` `&=` `^=` `|=`
11. `,`

The main difference is that bitwise operations and shift has higher precedence than
addition and multiplication in C2. Bitwise operations also have higher precedence than
the relational operators. There is also no difference in precedence between && || or
between the bitwise operators.

###Examples

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

The change of precedence of bitwise operators corrects a long standing issue in the C
specification. The change in precedence for shift operations goes towards making the
precedence less surprising.

Conflating precedence of || with &&, relational with equality operations, and bitwise
operations with each other is motivated by simplification: few remember the exact
internal differences in precedence between bitwise operators (to take an example).
Left-to-right offers a very simple model to think reason about the internal ordering,
and encourages use of explicit ordering where best practice in C is to use parenthesis anyway.

