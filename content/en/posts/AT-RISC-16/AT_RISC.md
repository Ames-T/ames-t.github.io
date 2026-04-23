+++
title = 'AT RISC Processor'
date = 2026-03-01T21:00:00Z
draft = false
math = true
tags = ["AT-RISC"]
toc = true
+++

## AT-RISC Processor Motivations.

For many years I have wanted to make my own processor, not just in Verilog but a physical, breadboard or PCB processor.

After previously simulating a SAP-1 processor on Logisim, I have become keen to make a more advanced "toy" processor. This is where AT-RISC comes in.

AT-RISC is a fun play on my initials (AT) and the term RISC, which stands for Reduced Instruction Set Computer. I sometimes end the AT-RISC title with 16, to show that it is a 16-bit processor.

## Inspirations

I have a whole load of inspirations for this processor, but I will list the main few.

[**Ben Eater**](https://www.eater.net) - One of the Homebrew Processor icons, at least on YouTube, his SAP series was the first one I got really invested in, I spent many years trying to find an affordable physical copy of *Malvino & Brown's - Digital Computer Electronics* which is the book he based a lot of his designs from.

[**Fabian Schuiki**](https://fabian.schuiki.ch/) - Has an impressive YouTube series on his superscalar processor. He is actually an Engineer at SiFive which I also find very interesting.

[**DipDot**](https://www.youtube.com/@dipdoting) - Has build a gorgeous relay processor, I am a really big fan of his backplane design, however I do not plan on using a similar design.

[**James Sharman**](https://www.youtube.com/@weirdboyjim) - Has an unbelievably underrated and long running series on his jam-1 processor, I am heavily inspired by his PCB layout design. As well as his work on IO for his processor.

## My Specification

There is no point in setting *too* much in stone as fas as the design at the moment, however there are some key ideas I wish to follow.

**RV32I Style Instructions** - I am very keen on making the instructions set practically the same as RV32I, of course I will not be following the structure 1:1.

**RISC Style Pipeline** - Classic RISC pipelines are split into 5 stages *Fetch, Decode, Execute, Memory, Writeback*, I intend on following this pipeline style.

**TTL Style Components** - This one is pretty obvious. Using as many 74 series chips as possible.

**PCB Based Design** - Modularising the deign as much as possible, via a bunch of PCBs on a series of backplanes. I refer to Fabian Schuiki and James Sharmans designs.

## Architecture

I won't go into too much detail, but I will include my current sketch of the processor architecture. As previously stated it will follow a RISC-Style pipeline.

![Architecture](AT-RISC/Architecture.png)

It is worth noting that this diagram is incomplete and will evolve as this project grows.

## Build process

The build process for this project will aim to follow the following steps

1. Conceptualisation
	- Come up with module idea, goals, and design limitations.

2. Verilog design
	- Write the module in Verilog with a test suite.

3. Breadboard Design
	- Produce on breadboard if possible, Some stuff will not be feasible to make, due to the 16-bit word size.

4. Schematic Design
	- Produce Detailed Schematics

5. PCB Layout
	- Produce PCB, using SMD components and pin headers.

6. Test using Verilog Test Suite
	- Using a FPGA to test the modules, as well as testing module clock speeds

7. Write up!
	- Write a blog post about it, and also document my design.
