+++
title = 'Homebrew Processor - Project Concept'
date = 2026-02-08T21:09:15Z
draft = false
math = true
tags = ["homebrew-processor"]
+++

For many years now, I have been keen to expand my low-level computing knowledge, ever since my electronics teacher told me it's possible to make a processor purely out of NAND gates.
Today I plan to embark on my first project down this rabbit hole.

## Premise

A home-brew breadboard style digital processor using some of the following mediums, under different levels of abstraction:

This project is greatly inspired by projects such as Ben Eater's 8-Bit breadboard computer and various relay computers by people such as Paul Law and Dipdot (Both Dr Harry Porter style)

## Mediums

- RTL, using Verilog on a FPGA
	- Requires no new parts and can be simulated on computer
	- Uses my existing Cmod A7-35T
	- Expands my knowledge of RTL

- TTL, initially breadboard designed, followed by a single PCB
	- Relatively low investment (Breadboards, Logic chips, Wire)
	- Reasonably Possible project at my level of knowledge
	- Build modular, use the FPGA model to test individual components

- Transistors, develop on breadboard, then on large set of modular PCBs
	- Large investment (Thousands of transistors, Custom PCBs, Enclosures, etc)
	- Difficult yet possible goal, with perseverance
	- Goal of ending up in a modular 10-inch rack, like deskpi's rack. Uses backplane bus PCBs
	- Will require a bit size of 8, compared to the 16 planned for the other two mediums
	- Again, build modular, use the FPGA model to test individual components

- Relays, develop on breadboard, then on large set of modular PCBs
	- Much, much larger investment
	- Much more difficult, somewhat unrealistic
	- Would make rather cool noises (Massive plus)
	- Again, would require smaller bit size. 8 or even 4

## Starting Point

My entry point into this project is firstly to build a fully functioning FPGA implementation, which will ensure the concept works and allow for streamlined development of the other mediums. This FPGA implementation will be 16-bit but be scalable for testing with for 8-bit transistor implementation. 