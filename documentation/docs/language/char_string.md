# Strings and Characters

C2 has string and character like C and are very similar.

## Character literals

Character literals are used like this:

```c
char c = 'a';
```


## String literals

String literals are used like this:

```c
char* s = "the quick brown fox\n";
```


## Escape sequences:

C2 defines a number of *escape sequences* on a character or string literal:

* `\"`
* `\'`
* `\?`
* `\\`
* `\a` - audible bell
* `\b` - backspace
* `\f` - new page/form feed
* `\n` - newline
* `\r` - carriage return
* `\t` - tab
* `\v` - vertical tab
* `\xXX` - where XX is a hexadecimal number (eg. \x12)
* `\ooo` - where ooo is an octal number (eg. \177)

