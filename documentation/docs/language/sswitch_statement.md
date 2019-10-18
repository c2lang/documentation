
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
* there is no fallthrough
* break can be used like in switch statement


