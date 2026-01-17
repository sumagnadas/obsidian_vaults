## Miscellaneous
---
### .rept
```assembly
.rept n
instruction_1
instruction_2
...
instruction_m
.endr
```
A GNU assembler directive to repeat the block of instructions inside the `.rept`->`.endr` block `n` times.
### nop
- No-operation single-byte instruction
- Can be used as filler between instructions
## Interesting Examples
---
### Adding directly to memory
```assembly
add qword ptr [0x404000], 0x1337
```
You can do this but you have to mention the size of the memory location like is it 64-bit (`qword`), 32-bit (`dword`) 16-bit (`word`) or 8-bit (`byte`). `ptr` is just a size directive.
### Storing fewer bits (64 bits from 0x12345 -> only lower 32 bits in 0x133337)
```assembly
mov rax, 0x12345
mov rbx, [rax]
mov rax, 0x133337
mov [rax], ebx # only the lower 32 bits will be stored
```
### Writing strings by moving individual bytes to stack
```assembly
mov BYTE PTR [rsp+0], '/'
mov BYTE PTR [rsp+1], 'f'
mov BYTE PTR [rsp+2], 'l'
mov BYTE PTR [rsp+3], 'a'
mov BYTE PTR [rsp+4], 'g'
mov BYTE PTR [rsp+5], 0
```
## Moving values around
---
### Square brackets/dereferencing
```assembly
inc [rax] # address in register
or
mov [0x123456], 0x5af3 # direct address
or
mov rdi, [rax+offset] # offset from register
```
Square brackets can be used to dereference an address and access the data stored at that memory location. We can dereference an address stored in register or directly. We can also add an offset to it and access the memory after that.
*Note:* We can use the square brackets to access the individual bytes, words, etc by moving to a partial register and offsetting the location. For example, this code accesses the 5th byte stored at the address
```assembly
mov al, [address+4]
```
*Note:* We can do address calculation like
  ```assembly
  mov rax, 0
  mov rbx, [rsp+rax*8]
  inc rax
  mov rcx, [rsp+rax*8]
  ```
  This is a basic stack traversal in assembly. (Remember, stack grows increasingly/to the right). Address calculation also has limits -> the calculation can only be in the form of `reg+reg*(2/4/8)+val`.
### mov
```assembly
mov dest, src
mov rax, 0x539
```
This instruction copies data from `src` to `dest` where both `src` and `dest` are memory locations/registers. (Only src can be a constant value.)
Also you can `mov` to partial registers mentioned above.
	*Note:* `mov`-ing to a 32 bit partial zeroes out the rest of the register before moving unlike the other partials because of performance reasons.

- You can also `mov` partials to another partial register as long as their length are same or they get trimmed or extended accordingly
- `mov` can be used to move data from memory addresses to registers by using the square bracket notation.
```assembly
mov rax, 0x12345 
mov [rax], rbx # this moves data to the location pointed by rax
mov rbx, [rax] # this moves data from the location pointed by rax
```
- Writing/`mov`-ing immediate value straight to memory pointed out by a register requires specifying its size.
  ```assembly
  mov rax, 0x133337
  mov DWORD PTR [rax], 0x1337
  ```
  This is required because 0x1337 can mean 0x1337 or 0x00001337 hence specifying size tells the assembler whether it requires padding or not. (PTR might not be reqd)
- Data is stored as little endian when storing in memory. Hence **bytes are shuffled during multi-byte storage from registers to memory. Individual bits are not shuffled, only bytes during multi byte storage.** For this reason, **writes to the stack behave just like any other write to memory** since it's basically pushing into memory.
  *Note:* Little endian is basically storing the data as right to left, i.e., the storing of data starts from the last bit(LSB) and moves towards the first bit (MSB).
- We can also do `rip`-relative addressing to access the memory next to code but generally its not writable.
##### Extending and shrinking data
You can extend data from a smaller register to bigger register for bigger calculations using `mov`
```assembly
mov bh, 0xf3
mov rax, bh
```
Loading fewer bits is also similar (this example shows pulling loading from memory)
```assembly
mov rax, 0x133337
mov bh, [rax] # will trim according to the destination size
```

### movsx
```
movsx rax,eax
```
`mov` doesn't preserve signs when extending data from a smaller partial register, like `eax` to a bigger (probably partial) register, like `rax`. For example,
```assembly
mov eax, -1 # eax contains 0xffffffff (4294967295 unsigned or -1 signed)
mov rax, eax # rax will contain 0x00000000ffffffff (4294967295)
```
Now `mov` will copy it like normal and signs will not be preserved properly.
`movsx` will fill the upper half with the top bit of the smaller register to preserve the signs i.e. the Two's Complements.
```assembly
mov eax, -1 # eax contains 0xffffffff (4294967295 unsigned or -1 signed)
movsx rax, eax # rax will contain 0xffffffffffffffff (18446744073709551615 unsigned or -1 signed)
```
### push
```assembly
push rax # pushes value of the register to the stack
push 0x1337 # pushes the value 0x1337 to the stack
```
Pushes value to the stack.
Subtracts 8 from rsp (RSP grows to the right i.e. towards smaller memory addresses)
*Note:* You can only `push` max 32-bit values on the stack on 64-bit x86 architecture. In other cases, its the word width of the architecture.
```assembly
sub rsp, 8
mov [rsp], 0x12345
```
equivalent to
```assembly
push 0x12345
```
### pop
```assembly
pop rax # pops the top value of the stack into rax
pop rcx # pops the top value of the stack into rcx
```
Pops value from the stack
Adds 8 from rsp
### lea
```assembly
lea reg3, [reg1+reg2*(2/4/8)+val]
```
This instruction moves the address of the memory location of the second address into reg3.
## Arithmetic
---
### add
```assembly
add rax, rbx
```
Adds `rax` and `rbx` and stores it in the first operand mentioned i.e. `rax` in this case
### sub
```assembly
sub rax, rbx
```
Subtracts `rbx` from `rax` and stores it in the first operand mentioned i.e. `rax` in this case
### imul/mul
```assembly
imul rax, rbx
```
Multiplies `rax` and `rbx` and stores it in the first operand mentioned i.e. `rax` in this case. `imul` is signed multiplication while `mul` is unsigned.
### div 
```assembly
div rbx
```
`div` actually divides a 128-bit dividend by a 64-bit divisor. The above instruction is actually equal to 
```
rax = rdx:rax / rbx
rdx = remainder
```
`rdx` holds the upper 64-bit of the dividend while `rax` holds the lower half. When dividing, be careful of this fact as `div` will always divide like this so `rdx` being nonzero will cause issues.
*Note:* Division here is purely integer math. Float divisions are rounded down to the nearest smaller integer.
### inc
```assembly
inc rax
```
Increments `rax` by 1
### dec
```assembly
dec rax
```
Increments `rax` by 1
### neg
```assembly
neg rax
```
Negates `rax`
### not
```assembly
not rax
```
Negates each bit in `rax`
### and
```assembly
and rax, rbx
```
Performs bitwise & operation between bits of `rax`, `rbx`
### or
```assembly
or rax, rbx
```
Performs bitwise | operation between bits of `rax`, `rbx`
### xor
```assembly
xor rax, rbx
```
Performs bitwise ^ operation between bits of `rax`, `rbx`
### shl
```assembly
shl rax, 10
```
Shifts the bits of `rax` left by 10, filling the right newly empty space with 10 zeroes
### shr
```assembly
shr rax, 10
```
Shifts the bits of `rax` right by 10, filling the left newly empty space with 10 zeroes
### sar
```assembly
sar rax, 10
```
Shifts the bits of `rax` right by 10, filling the left newly empty space with the previous leftmost bit to preserve sign i.e. sign-extension.
### ror
```assembly
ror rax, 10
```
Rotates the bits of `rax` left by 10
### rol
```assembly
rol rax, 10
```
Rotates the bits of `rax` right by 10

## Flow control 
---
### jmp
```assembly
mov cx, 1337
jmp LABEL1
mov cx, 0 # this will not be executed ever
LABEL1:
push rcx
```
`jmp` is used to perform an unconditional jump to a label. This basically skips the next instruction going straight to the mentioned label.
*Note:* You HAVE to place the address you want to jump to inside a register for `jmp` to work, if you want absolute jumps instead of a label.
In memory, the `jmp` instruction is given as 

| instruction | bytes to skip |
| ----------- | ------------- |
| eb          | 04            |
In that regard, the instruction `eb fe` will cause an infinite loop as its saying to jump(`eb`) -2 bytes (`fe`) which will get to start of the instruction.
### cmp
```assembly
cmp rax, rbx # between registers
# or
cmp rbx, 0 # between register and value
```
This is used to compare the values of the two operands. `cmp` sets certain flags on `rflags` register to tell whether there was a carry (CF), was the result zero (ZF), did the result overflow or wrap between positive and negative (OF) or if the result was a signed integer i.e. had its signed bit set (SF).
These flags are used by the conditional jump statements to control flow.
### test
```assembly
test rax, rax
```
This is used to see if the `rax` is zero.
### Conditional jumps
All of the following jump instructions follow the syntax of `jmp`

| instruction | purpose                                    | flags they check(not that important) |
| ----------- | ------------------------------------------ | ------------------------------------ |
| `je`        | jump if equal                              | ZF=1                                 |
| `jne`       | jump if not equal                          | ZF=0                                 |
| `jg`        | jump if greater                            | ZF=0 & SF=OF                         |
| `jl`        | jump if lesser                             | ZF=0 & SF!=OF                        |
| `jle`       | jump if lesser than or equal               | ZF=1 & SF!=OF                        |
| `jge`       | jump if greater than or equal              | SF=OF                                |
| `ja`        | jump if above (unsigned `jg`)              | CF=0 & ZF=0                          |
| `jb`        | jump if below (unsigned `jl`)              | CF=1                                 |
| `jae`       | jump if above or equal<br>(unsigned `jge`) | CF=0                                 |
| `jbe`       | jump if below or equal<br>(unsigned `jle`) | CF=1 or ZF=1                         |
| `js`        | jump if signed                             | SF=1                                 |
| `jns`       | jump if not signed                         | SF=0                                 |
| `jo`        | jump if overflow                           | OF=1                                 |
| `jno`       | jump if not overflow                       | OF=0                                 |
| `jz`        | jump if zero                               | ZF=1                                 |
| `jnz`       | jump if not zero                           | ZF=0                                 |

## Functions
---
### Normal functions
```assembly
mov rdi, 0  
call FUNC_CHECK_LEET  
mov rdi, 1  
call FUNC_CHECK_LEET  
call EXIT

FUNC_CHECK_LEET:  
  test rdi, rdi  
  jnz LEET  
  mov ax, 0  
  ret  
  LEET:  
  mov ax, 1337  
  ret
```
Functions can be written in assembly code using `call` and `ret`. The functions are basically labels which you can `call` and then `ret` returns to the address from where it was called.
Now functions uses a convention for getting its arguments. This is different from architecture to architecture.
Linux amd64, the architecture we are using passes its arguments in `rdi`, `rsi`, `rdx`, `rcx`, `r8`, `r9` and gets its return value in `rax`.
Now value of some registers are saved by the functions (on a stack different from `rsp`) when called and, even after using those registers for calculations, when the function returns, it restores the original values of these registers. `rsp` is also "saved" by the function, obviously. The other registers have no such protection and you will have to save the values on your own to get them back when the function returns.
### Syscalls
Syscalls are how programs interact with CPU. Syscall basically calls a system function that serves a specified purpose. For that purpose, the following registers are used to provide info to `syscall`
- `rax` -> provides the `syscall` function via a number which maps to a specific function.
- `rdi`, `rsi`, `rdx`, `r10`, `r8`, `r9` -> provides arguments to the `syscall` (in order from 1st to  6th argument)
- `rax` also holds the return value of the function
#### exit(int ret_code) 
```assembly
mov rdi, 2
mov rax, 60
syscall
```
- code => 60
- Exits the program with return code `ret_code`
#### read(int fd, char \*buf, size_t count)
- code => 0
- Reads `count` bytes  from `fd` to memory location `buf`
- Returns the number of bytes read from `fd`
#### write(int fd, char \*buf, size_t count)
```assembly
mov rdi, 1 # stdin file desc
mov rsi, rsp # write data from the stack (rsp is the stack pointer location)
mov rdx, rax # number of bytes to write
mov rax, 1
syscall
```
- code => 1
- Output `count` bytes  from memory location `buf` to `fd`.
- Returns the number of bytes outputted to `fd`.
#### open(char \*path, int flags)
```assembly
mov rdi, rsp # get filename from the stack
mov rsi, 0 # read-only flag
mov rax, 2
syscall
```
- code => 2
- Read file `pathname` wrt to `flags`
- Returns the new file descriptor
#### socket(int domain, int type, int protocol)
```assembly
mov rdi, 2 # 2 for AF_INET
mov rsi, 1 # 1 for SOCK_STREAM, 2 for SOCK_DGRAM
mov rdx, 0 # 0 for IPPROTO_IP
mov rax, 41
syscall
```
- code => 41
- Creates a socket to communicate via the network mentioned by `domain`, following the protocol mentioned by `protocol`.
- Returns a new file descriptor to use with `bind` and `accept`
- Possible values for type starts with `SOCK_` and that of domain starts with `AF_`.
#### bind(int fd, struct sockaddr \*addr, socklen_t addrlen)
```assembly
mov rdi, 3 # Assuming the FD returned by socket is 3
mov rsi, [rdx] # Assuming rdx stores the address of the sockaddr object
mov rdx, 16 # sockaddr object len for IPv4 is 16 (might not be the correct thing)
mov rax, 49
syscall
```
- code => 49
- Binds the socket defined by the `fd` to an actual connection defined as per the  `sockaddr` object whose length is `addrlen`
- The `sockaddr` has a general form of 
  ```c
	struct sockaddr{
		uint16_t sa_family;
		uint8_t sa_data[14]
	}
	```
  This struct passes the data for the socket to `bind` to an actual network interface. For `sa_family=AF_INET` i.e.  IPv4, the struct is packed in the form of
  ```c
  struct sockaddr_in{
	  uint16_t sin_family;
	  uint16_t sin_port;
	  uint32_t sin_addr;
	  uint8_t __pad[8];
  }
  ```
- Although the first argument is passed as-is via `mov`, the `sockaddr` object has to be passed minding the fact that normally storing values in the memory writes them in little-endian order but network interfaces read in big-endian order.
#### listen(int fd, int backlog)
```assembly
mov rdi, 3 # Assuming the FD returned by socket is 3
mov rsi, 0 # maximum number of queued connections at a time
mov rax, 50
syscall
```
- code => 50
- Marks the socket as ready for `listen`-ing and accepting incoming connections at this address of the network interface for the device. `backlog` sets the maximum number of queued connections at a time.
#### accept(int fd, struct sockaddr \*addr, socklen_t \*addrlen)
```assembly
mov rdi, 3 # Assuming the FD returned by socket is 3
mov rsi, 0 # Currently moving nullptr as per first encounter with this syscall 
mov rdx, 0 # Currently moving nullptr as per first encounter with this syscall
mov rax, 43
syscall
```
- code => 43
- Accepts the first incoming connection in the connection queue for the socket pointed to by the `fd`.
- Returns a new FD for the connection.