+++
title = 'Homebrew Processor - ALU Design and Implementation'
date = 2026-04-04T21:00:00Z
draft = false
math = true
tags = ["homebrew-processor"]
+++

## ALU design

As mentioned in my previous post (Over a month ago now), the core of this design is the ALU. Since my last post I have made a lot of progress with the design and even implemented some of it on the breadboard and in Verilog.

This post will be focused on the Verilog design of the ALU and some initial plans/thoughts for the breadboard implementation.

It's also worth noting at this point I have made many conceptual overhauls to this project. I won't particularly be going over these in this post, but in my physical experiments I have figured out some impracticalities about the previously stated design, we will address these when we come to these.


### Verilog Implementation

We start off with the definition of this module. As previously described there are the two input registers (Labelled A and B in this case), the Opcode input, the Output enable (High to drive the bus), the flag registers (Zero, Carry, Overflow and Sign) and finally the tristate bus In/Out.

This Verilog block is pretty simple and straightforward.

```verilog
module alu (
    input wire [15:0] A,
    input wire [15:0] B,
    input wire [3:0] alu_op,
    input wire out_en,

    output reg zero_flag,
    output reg carry_flag,
    output reg overflow_flag,
    output reg sign_flag,

    inout wire [15:0] bus
);
```

We also have these following internal registers/wires. These just allow for the internal functions which will then be assigned to external.

```verilog
    reg [15:0] result;
    reg carry;
    reg overflow;
```

Now for the big and juicy bit, the actual operations. Now, this block is quite big but it's relatively simple, this boils down to a case statement that sets the results register to the result of whatever operation based on the opcode. It's also resetting the registers to 0 on every cycle to make sure the flag is only set if the opcode requires it.

```verilog
// ALU operations
    always @(*) begin
        carry = 0;
        overflow = 0;

        case (alu_op)
            4'b0000: result = A;                    // PASS A
            4'b0001: result = B;                    // PASS B
            4'b0010: begin                          // ADD
                {carry, result} = A + B;
                overflow = (A[15] == B[15]) && (result[15] != A[15]);
            end
            4'b0011: begin                          // SUB
                {carry, result} = A - B;
                overflow = (A[15] != B[15]) && (result[15] != A[15]);
            end
            4'b0100: result = A & B;                // AND
            4'b0101: result = A | B;                // OR
            4'b0110: result = A ^ B;                // XOR 
            4'b0111: result = ~A;                   // NOT A 
            4'b1000: begin                          // SHL
                result = A << 1;
                carry = A[15];
            end
            4'b1001: begin                          // SHR
                result = A >> 1;
                carry = A[0];
            end
            4'b1010: begin                          // ROL
                result = {A[14:0], A[15]};
                carry = A[15];
            end
            4'b1011: begin                          // ROR
                result = {A[0], A[15:1]};
                carry = A[0];
            end            
            4'b1100: begin                          // INC
                {carry, result} = A + 1;
                overflow = (A == 16'h7FFF);
            end
            4'b1101: begin                          // DEC
                {carry, result} = A - 1;
                overflow = (A == 16'h8000);
            end
            4'b1110: begin                          // NEG
                result = -A;
                overflow = (A == 16'h8000);
                carry = (A != 0);
            end
            4'b1111: result = 16'b0;                // ZERO

            default: result = 16'b0;                // ZERO
        endcase
    end
```

And finally, we have the flag updating block. These just constantly update the flag module connections to the correct value. Notice how this is the first instance of the zero and sign flag, these do not require setting based on the operation.

The final line drives the bus with the value of the result if the output enable is held high, if it isn't it's held floating by this module, this is best as it avoids bus contention.

```verilog
    // Flags
    always @(*) begin
        zero_flag  = (result == 16'b0);
        carry_flag = carry;
        overflow_flag = overflow;
        sign_flag = result[15];
    end

    // Bus output
    assign bus = (out_en) ? result : 16'bz;

```

### Physical Implementation
The physical implementation of this module is fairly straightforward. It gets split into two sections.


#### ALU Block 
The ALU block contains all the arithmetic and logical operators (excluding bitshifting) This block is made of 4x National semiconductor DM74181Ns, these are magical little chips which pack so much in them, but I will be using 12 of the operations.

These chips have a handful of key inputs. Opcode (Selection bits), Mode (Logic or Arithmetic) and the Carry input.
Between the chips the Selection bits and the Mode bit is shared between them all, with the carry chained through all four of them, the carry in flag is managed by the first chips in the series.

| Operation | OPCODE | S      | C   | L |
|-----------|--------|--------|-----|---|
| PASS A    | 0000   | 0000   | 0   | 0 |
| PASS B    | 0001   | 0101   | X   | 1 |
| A + B     | 0010   | 1001   | 0   | 0 |
| A - B     | 0011   | 0110   | 1   | 0 |
| A AND B   | 0100   | 1101   | X   | 1 |
| A OR B    | 0101   | 1000   | X   | 1 |
| A XOR B   | 0110   | 0110   | X   | 1 |
| NOT A     | 0111   | 0000   | X   | 1 |
| A + 1     | 1100   | 0000   | 1   | 0 |
| A - 1     | 1101   | 1111   | 0   | 0 |
| -A        | 1110   | 1001   | X   | 1 |
| 0         | 1111   | 1100   | 1   | 0 |

These operations were selected to give a range of key basic functionalities, as well as good advanced operations.

#### Bitwise Block
The Bitwise block handles the logical bit shifting/rotating.

This block is comprised by 4x 74194 bidirectional shifting chips, as well as one 74157 multiplexing chips to switch between rotating and shifting.
Due to the design of the 74194 chips, I will, at a later date attempt to implement microcode loops to allow for shifting $n$ times, rather than just the once, allowing for more computational efficiency.

I'm not sure if this layout is clumsy or clean, but it's the tidiest I got it after plenty of logic simulations.


#### Bus Driving
Both these blocks outputs will be driven by 2x 74245 Octal Tri-State bus drivers, in fact pretty much everything that drives the bus will be using these, including the registers (next post)


#### Microcoding
Both these blocks will be driven by the same microcode ROM, this will simply take the 4-bit Opcode from the main processor control and coordinate the inputs of the two blocks to give the desired output, hence the combined unit will only have the Opcode, Carry and Bus driving control signals. Allowing for it to only use 6 bits of the control word.


### Breadboard version
The breadboard version is well on it's way, however it is very quickly becoming a mess of wires. Having it split into two blocks is very handy for wiring but physical access to the A and B registers proves messy.

## Final Thoughts
Overall, I am quite proud of this ALU design, I could have challenged myself more by skipping the 74181 chips and instead implement the operations using adders and logic chips, however the allure of more operations allowed for a more challenging programming end of the project.

The source for this project, as well as tests are in this [Github repository](https://github.com/Ames-T/AT16).

