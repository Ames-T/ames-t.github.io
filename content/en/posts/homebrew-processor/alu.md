+++
title = 'Homebrew Processor - ALU'
date = 2026-03-01T21:00:00Z
draft = false
math = true
+++

## ALU

The ALU or Arithmetic Logic Unit is the core of the computer. It performs arithmetic and logic functions.
The functions are selected by the opcode. These operations can be combined to form other operations allowing for endless functional possibilities, although it can be computationally inefficient when compared to having an ALU function baked in, but that costs transistors. I will go into the operations I intend to implement later on in this post, for now I will go into the general structure of the ALU.

### Inputs and Outputs

- The ALU has 2 register inputs, $B$ and $C$, I chose to have the input registers hardwired instead of opting for register selection. This is to maintain some simplicity, it is an option I may implement in the future.

- The ALU also has the opcode input, this comes directly from the control unit and tells the ALU what to do with it's inputs. As I've previously mentioned the architecture I've based this on uses a 3-bit opcode, I will instead opt for a 4-bit opcode, this allows for much more functionality ($16$ vs $8$ operations).

- The ALU outputs it's result directly to the data bus, some ALU designs have an intermediary bus before writing to the data bus, I will not be implementing that at this time.

- The ALU also has four 1-bit flags;
	- $Z$ - Zero flag, $1$ when the ALU result is 0
	- $C_y$ - Carry flag, $1$ when the ALU operation results in a bit being carried
	- $S$ - Sign flag, Also the $MSB$ of the result, $1$ if the number is negative. Irrelevant when number is unsigned
	- $V$ - Overflow flag, similar to the Carry flag, however used in the case of signed numbers
	 
	 These flags are very useful for acting on data.

### Operations

The operations I have in mind are:

#### Arithmetic

| Operation | Result |
|-----------|--------|
| ADD       | $A+B$  |
| SUB       | $A-B$  |
| INC       | $A+1$  |
| DEC       | $A-1$  |
| NEG       | $-A$   |

#### Logic

| Operation | Result     |
|-----------|------------|
| AND       | $A\land B$ |
| OR        | $A \lor B$ |
| XOR       | $A\oplus B$|
| NOT       | $\neg A$   |

#### Bitwise

| Operation      | Result          |
|----------------|-----------------|
| SHL            | $A << 1$        |
| SHR            | $A >> 1$        |
| ROL            | ROTATE $A$ RIGHT|
| ROR            | ROTATE $A$ LEFT |

#### Data Movement

| Operation | Result |
|-----------|--------|
| PASS A    | $A$    |
| PASS B    | $B$    |
| ZERO      | $0$    |

### Operation Justifications
These operations have a reasonable range of computational capability without massively increasing hardware complexity.
If I do create a less abstracted processor, This set will be reduced to NOP, ADD, INC, AND, OR, XOR, NOT, SHL.


### OPCODES

| OPCODE | OPERATION |
|--------|-----------|
| $0000$ | PASS A    |
| $0001$ | PASS B    |
| $0010$ | ADD       |
| $0011$ | SUB       |
| $0100$ | AND       |
| $0101$ | OR        |
| $0110$ | XOR       |
| $0111$ | NOT A     |
| $1000$ | SHL       |
| $1001$ | SHR       |
| $1010$ | ROL       |
| $1011$ | ROR       |
| $1100$ | INC       |
| $1101$ | DEC       |
| $1110$ | NEG       |
| $1111$ | ZERO      |