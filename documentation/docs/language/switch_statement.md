
# Switch statement

The switch statement is similar to C, except for the changes below:

    * no auto-fallthrough
    * auto-scoping
    * default (if present) must be last

## Auto-fallthrough

Unexpected fallthrough is a big source of issues in C programs, so in C2
this implicit behaviour has been removed. Any case statement must end in
either: *break* | *fallthrough* | *return* | *continue* | *noreturn-func*.
The fallthrough statement can only appear at the top-level of the case body,
not in some sub-expression (eg. _if (x) { fallthrough; }_ is not allowed).

```c
   switch (i) {
      case 0:  // this gives an error, last statement of a case must be one of
               // break/fallthrough/continue/return.
      case 2:
         fallthrough;    // will fallthrough
      case 3:
         break;
      default:
         fallthrough; // not allowed in last case
    }
```

## Case auto-scope

```c
    switch (i) {
        case 1:
            i32 a = 10;     <- in C, user would have to add {} around case body.
            return calc(a);
        case 2:
            i32 a = 20;     <- no clash, since it's another scope
            ...
    }
```

## Default last

Finally, the default statement must be last, to increase code-uniformity.


# Sswitch statement

C2 introduces a new statement: *sswitch* that can be used to
make a switch-like stament, but with strings (sswitch = string switch).

```c
func void handleCommand(const i8* cmd) {
   sswitch (cmd) {
   case nil:
      return;
   case "start":
      return;
   case "stop": // NOTE: NO fallthrough!
   case "move":
       break;
   default:
       return 10;
   }
```

Rules for using sswitch:

* empty sswitch (no cases/default) is not allowed
* default case (if present) must be last
* case argument must be a *string literal* or *nil*
* there is no fallthrough (fallthrough keyword cannot be used)
* break can be used like in switch statement


