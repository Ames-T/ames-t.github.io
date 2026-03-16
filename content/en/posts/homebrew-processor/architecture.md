+++
title = 'Homebrew Processor - Architecture'
date = 2026-02-15T21:00:00Z
draft = false
math = true
+++

As discussed in my [previous post](posts/homebrew-processor/concept/) I am starting work on my homebrew 16-bit processor. The fist step on this journey is to pick an architecture.

## Options

I have been exposed to multiple architectures in my research. A common option used is the Simple-As-Possible architecture described by (Malvino and Brown 1994) in Digital Computer Electronics, which is used by [Ben Eater](https://eater.net/). Unlike this, however, I have chosen to follow a route [Paul Law](https://www.relaycomputer.co.uk/) (Who's blog inspired these posts) followed with his 8-bit relay computer by modifying the architecture created by Dr Paul Law

## Architecture

The basic Porter architecture is as drawn:

![Architecture](HomebrewProcessor/Architecture.png)

In this version, it's specifications will be as follows:

- 16-Bit Data bus (Can be scaled to 8-bit in less abstracted builds)
- 16-Bit Address bus
- 6 $\times$ 16-bit general purpose registers
	- In the case of an 8-bit system registers $XY$ and $M$ can be split into two 8-bit registers.
- 16-Bit ALU, with three flag registers (Zero, Carry and Sign)
- 16-Bit Jump register
- 16-Bit Instruction register
- 16-Bit Program Counter
- 32k 16-bit SRAM

## Design Justification

I believe this architecture has both relative simplicity, and scalability. The number of registers allows for modularity as well as more computational options. The Jump register is very handy and takes the processor from being really quite limited to actually quite functional.

Unlike Dr Harry Porters build, I will be increasing the number of functions on the ALU, using 4 bits instead of 3, from $2^3$ options to $2^4$, however, if I was to make a relay version (unlikely) I would reduce the operation set.