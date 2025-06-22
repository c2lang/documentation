## Raw Strings

Sometimes a large piece of text/code needs to be inlined. To facilitate this, C2 adds
*raw strings*

```c
const char[] Fragment =
    ```
    // THIS IS C CODE
    #include <stdio.h>

    int main(int argc, char* argv[]) {
        printf("Hello C\n");
        return 0;
    }
    ```;
```

Rules:

* the number of starting backticks (`) must match the number of trailing backticks
* The indentation of the starting backticks counts as the *zero-column*
* The trailing backticks must have the same indentation as the starting backticks
* Nothing inside the raw-string is evaluated (#ifdef etc)
* Raw strings can be combined with regular strings

