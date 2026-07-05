## RISC-V
---
### Registers
![[Pasted image 20260215230655.png]]
### ISA
Each instruction in RISC-V has a size of 4 bytes or 32 bits
#### Instruction types and structure
First 3 byte structure =>  
The first 3 bytes denote the type of instruction -
- R-type => 0110011 => 51
- I-type  => 0010011 => 19
- L-type => 0000011 => 3
- S-type => 0100011 => 35
- B-type => 1100011 => 99
**Note:** `func3` and `func7` define the exact instruction that is being executed. `func3` tells about the kind of operation that is performed by the instruction. `func7` tells about if this is a different mode of the same operation. For example, `func3` for both `add` and `sub` is 0, but `func7` for sub is `0x20` while that of `add` is 0. This alludes to the fact that `add` and `sub` both use the same datapath(?) but for `sub`, you have to negate it before sending it through the datapath(?). Basically, `sub` = negate -> `add`. 
##### R-type
![[Pasted image 20260210010107.png]]
These type of instructions are for doing arithmetic operations between registers and storing to registers.
##### I-type
![[Pasted image 20260210010911.png]]
These type of instructions are for doing arithmetic operations between values in registers and immediate values.
##### I-type and S-type
![[Pasted image 20260210011017.png]]
These instructions are for data transactions, like loading and storing, between memory and registers.
##### B-type
![[Pasted image 20260210011151.png]]
These instructions are for branching/jump instructions.
#### Memory alignment
A CPU reads data in blocks or chunks, which depends on word length/data bus width. For example, if we need data from 0x00 to 0x04, it can be read in one CPU cycle. Instead, if we need data from 0x01 to 0x05, it will require two CPU cycles and also merging the two data parts into one part via bit shifting and masking. Hence misalignment requires extra work and also leads to crashes in some architectures.
### Control Unit and Datapaths
Datapath is a basic structure of a CPU, based on our instructions. It consists of block elements like ALU, MUX. It also consists of a control unit, which is responsible for controlling the MUXes and whether RegWrite signal is needed to be turned on i.e whether the register bank needs to be written into.
There are also blocks which depict Instruction Memory (RAM \[RX]), Registers (Register File), Data memory (also RAM \[RW]). 
- Register File is a representative structure for the registers, which receives 2 registers to read (?) and one register to write to as well as the write data. Whether the register file is being written into or read from is controlled by a control signal. **We can't read and write to a register in the same cycle.** 
- Data Memory is a block for writing or reading from memory i.e. RAM. It takes in an address to read from/write to and, in case of writing, the data to write. Whether the Data memory is being written to or read from is controlled by 2 different control signals.
Now, for ALU, the control signal ALU operation is actually a 4 bit signal which tells the ALU what to perform. (Assume that the ALU can only perform `add`, `subtract`, `AND`, `OR`).  For RISCV, its defined as such :-
![[Pasted image 20260218020950.png]]
ALUOp is the control signal from the control unit while the operation is another signal using a ALU control unit(?). Also the specifications for the ALUOp is such :-
- 00 => lw, sw
- 01 => beq (basically output is checked on the Zero output from the ALU which is given after `subtract`) 
- 10 => R-type here the funct3 and funct7 fields define the operations.
For the actual control signal i.e the operation :-
- 0010 => `add`
- 0110 => `subtract`
- 0000 => `AND`
- 0001 => `OR`
In case of `beq`, the Zero output also checked for checking if the default 4 offset has to be added to move PC forward or move to the branch target.
Below is given the full datapath for a simple processor :-
![[Pasted image 20260218020933.png]]
### Pipelining
The previous topic talked on instruction executing in single cycle. Executing each instruction sequentially is very inefficient and will take so much time.  What if each part of the instruction is executed in multiple clock cycles instead of the whole instruction like 
- Instruction fetch (IF),
- Instruction Decode (ID),
- Execution (EX),
- Memory access (MEM), 
- Write Back (WB)?
That is basically multicycle implementation. What this enables us to do is pipelining.

Pipelining is the process of running multiple instructions parallelly, i.e. at any given time, the block all or most of the blocks will be occupied with some part of a single instruction. For example, when the 1st instruction is in the ID stage, IF part of the 2nd instruction can start without causing any issues in the execution of the first instruction.