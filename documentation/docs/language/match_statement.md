
# Match statement

C2 introduces a new statement: *match* that can be used to
make a switch-like stament, but with strings.

```c
func void handleCommand(const i8* cmd) {
   match (cmd) {
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

Rules for using match:

* empty match (no cases/default) is not allowed
* default case (if present) must be last
* case argument must be a *string literal* or *nil*
* there is no fallthrough
* break can be used like in switch statement


