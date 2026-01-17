The following notes will contain notes about x86 assembly only for now.
## Registers
---
Registers are data storage for kind of immediate access while executing an instruction.
Registers can be divided into their lower halfs and accessed according to their historic names as follows :-
![[Pasted image 20251210011823.png]]
The last 16bits are divided into two accessible halfs -> H (high), L (low)
Some registers are special like `rip` -> Instruction Pointer
### rip
Stores the memory address of the next instruction in the program to be executed.
## Stack
---
The stack in assembly grows downward when pushing to it i.e. the address of the current top pointer decreases. The address of the current top pointer is stored in `rsp`. The address of the base of the stack is stored in `rbp`. 
Data is stored as little endian when storing in memory. Hence **bytes are shuffled during multi-byte storage from registers to memory. Individual bits are not shuffled, only bytes during multi byte storage.** For this reason, **writes to the stack behave just like any other write to memory** since it's basically pushing into memory.

*Note:* Although we are using the terms **top** and **base**, the base actually means the first stack address or the topmost address in this case while top here is the bottom-most address of the stack.
***NOTE:*** Never EVER write to `rsp` in a function just after calling. It contains the return address to the caller function.
***
## Instructions
---

For more details, check out [[Instructions]]
### Register Arithmetic
![[Pasted image 20251210020548.png]]
## Additional header information
---
Since we are using Intel syntax for x86 assembly, we need to add `.intel_syntax noprefix` which states the following :-
- We are following Intel syntax
- We are using the variant of that syntax which doesn't need extra prefixes to every instructions
Also, in general, every assembly code has a marked `_start` position to mark the start of the actual code bu you can omit that, in which case `ld` will give a warning stating "no `_start` found" and make an executable which starts executing from the start. If you want, you can prevent the warning by adding a 
```assembly
.global _start
_start:
...
```
## Endianness
---
Multi-byte primitives have their own format when stored in memory. The formats mostly tell us about how individual bytes are laid out in memory. It can be stored in big endian or little endian format.
#### Big Endian
This is how we see when we think about bytes being laid out. This is also how the data is stored in registers.
Imagine the data is `0x1f2e3d4c`. It will be stored in registers as

| b0  | b1  | b2  | b4  |
| --- | --- | --- | --- |
| 1f  | 2e  | 3d  | 4c  |
In Big Endian format, the MSB or leftmost byte is stored first and is laid out left to right in memory.
#### Little Endian
This is the exact opposite of Big Endian. The LSB or rightmost byte is stored first and then laid out from right to left in memory.
Imagine the data is `0x1f2e3d4c`. It will be stored in memory as

| b0  | b1  | b2  | b3  |
| --- | --- | --- | --- |
| 4c  | 3d  | 2e  | 1f  |
Endianness doesn't matter when we are working with what the CPU handles but when we have to interact with systems having data with different endianness, we have to convert the data to what CPU can handle and then work with it.
For example, if your CPU is little-endian and you have to interact with network protocols, you have to convert the data coming from the network interfaces from big-endian to little endian to work with it as network interfaces normally use big-endian format.
 *PS:* We can store small strings (string len $\le$ 8) straight to a memory location using registers.
 ![[Pasted image 20260116112601.png]]
 As we can see, we wrote "/flag" first straight to `rax` but we did so in reverse mode and then that value  to memory. As it will be written in little-endian mode, it will be reversed from the big-endian value in register and it will be written as "/flag" which we can then pass to a `syscall` or use accordingly.