+++
title = 'Homebrew Processor - Registers and Buses'
date = 2026-02-22T21:00:00Z
draft = false
math = true
+++

## Registers

A register holds a binary values for use in computation and data movement. Most registers are general purpose and can be both read and written, there are however some special registers that are read only, write only or different per bus. In the architecture diagram, the registers are the rectangles between the two buses.

In the 16-Bit version of this architecture, the general purpose registers are as follow;
- $A$ : General purpose. Read and Write on bus
- $B$ : General purpose. Read and Write on bus, one of two read by ALU
- $C$ : General purpose. Read and Write on bus, one of two read by ALU
- $D$ : General purpose. Read and Write on bus
- $M$ : In the case of 8 or 4 bit data bus, M is split into two general purpose registers that can be combined and read by the 16-bit bus. In the case of a 16-bit data bus, the M register is just one register that can be read by both the address and data buses
- $XY$ : Same as M, split into X and Y but the combined register can be both read and written to

As well as the general purpose registers, this architecture has a handful of special registers;
- $J$ : Similar to M and XY, J is a 16-bit Jump register, optionally split into two J registers that can only read from the data bus and the combined bus can only write to the address bus
- $Instruction$ : Holds the instruction currently being executed by the processor
- $PC$ : Holds the address of the next instruction to be executed
- $Z$, $C_y$, $S$, $V$ : 1-bit Flag registers for ALU output

## Buses

Buses are a set of wires that connects multiple parts of the computer. The architecture in the most abstracted versions of this processor is 16-bits wide.
Each bus can only hold one value at a time, so each part of the processor connected to said bus needs to wait it's turn to talk to the bus. In the architecture diagram the buses are the wide rectangles. This simplifies the diagram instead of showing 16 individual wires.

This architecture has two buses.
- 16-Bit Data bus.
- 16-Bit Address bus.

Each bus can hold different data at the same time.