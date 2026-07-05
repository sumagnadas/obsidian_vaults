You can use `set disassembly-flavor intel` to make `gdb` print assembly code in Intel syntax.
## Commands
---
### starti
Starts a program at the very first instruction
### ni/nexti
- Next Instruction
- Proceeds by running one machine instruction
- Skips going into `call` statements (functions) and straight up goes to the next instruction. (Complicated description: Stays at the same stack frame level)
### si/stepi
- Step Instruction
- Proceeds by running one machine instruction.
- Goes into `call` statements when encountered
### finish
- Runs a function until it returns and stops on the outer stack frame.
### print/p
- For printing variables, we use `$var`. In this way, we can also access register values using their names as variable names such as `$rax`, `$rip`.
- Allows C-style casting i.e. `*(long *) $rsp`
### x (examine)
`p` gives the actual value of the register/variable so, to get a dereferenced value interpreting it as something as given in format, we use `x`. The format can be specified as `x/<formatspec> $var`.
#### formatspec (works with both `p` and `x`)
- `<n>` => `<n>` is an integer providing how many values to print after it in the specified order. (always at front)
- `i` => gives the derefenced value as an assembly instruction. ***Useful for* `rip`**
- `g` => giant values or 64bit values
- `x` => hex values
- `u` => unsigned
- `d` => signed
- `a` => interpret them as addresses show where they point to..
### disassemble/disass
Can be used to dissassemble a function.
### info
Gives info about stuff
- `register`
- `break` (for getting all breakpoints)
### del
Deletes stuff (?)
- `break <id>`=> deletes the breakpoint with 
### set
Used to set value of a variable or declare a new one. Can be used to set value to memory locations via casting to `(unsigned long long *)` or to registers and so on after that.
- `set $var = 1234` => creates a new variable
- `set $rax = 0x23` => set a value to `rax` register.
- `set *(unsigned long long*)($rbp-0x8)=0x00007ffd65db13b0` => setting to a memory location pointed to by `[rbp-0x8]`
## Scripting
---
### commands
```gdb
b *main
commands
	silent
	p/a $rip
	continue
end
run
```
In scripting, we can run some commands when encountering a breakpoint. This can be done using `commands` => `end` block just after the breakpoint.
### silent
- Silences the breakpoint information printed when its hit.