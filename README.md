# Datapoint_2280 *
An exploration of how the Intel 8008, 8080, x86 and Zilog Z80 owe their heritage to the Datapoint 2200.

## Introduction

In late 1969 Victor Poor and Harry Pyle, whilst working for Computer Terminal Corporation (CTC), designed a TTL bit-serial processor to control their new Datapoint 2200 programmable serial terminal.

It allowed the terminal to be programmed in software to be functionally compatible with many of the mainframe and minicomputers that were popular at that time. One terminal platform could be sold to a very wide customerbase of computer users.

Poor and Pyle had met a few years earlier, as they were both enthusiatic Amateur Radio Hams. Pyle was barely out of college, and Poor was invited to join the new startup CTC as a senior design engineer.

Their respective Oral Histories were recorded by the Computer History Museum (CHM) in 2005:

https://www.computerhistory.org/collections/catalog/102658337

https://www.computerhistory.org/collections/catalog/102702017

Poor suggested building an 8-bit TTL processor, which could be incorporated into the base of the CRT serial terminal. 

It would scan the keyboard, store the ascii characters into memory, generate the video signal and control the CRT. Meanwhile it would receive the incoming serial data stream, store it to memory and format it for page display, whilst transmitting the User's text from a buffer back to the host mainframe or minicomputer.

It had twin cassette tape drives which were used for program and data storage. It completely removed the need for an electromechanical terminal and paper tape, such as the Teletype ASR33, which were slow, noisy and required regular mechanical servicing.

CTC's first product was 2 years earlier, the Datapoint 3300. It was one o the first "Glass Teletypes" to appear on the market. It was called the 3300 because it took up the same desktop footprint as a Teletype Model 33 - and it was 100 times better!

Calling the 2200 a serial terminal or "Glass Teletype" is something of an understatement, upon reflection it was really more akin to the first modern desktop personal computer.

https://en.wikipedia.org/wiki/Datapoint_2200

* Datapoint did not build a processor called the 2280, but the 2200 was released in 2 versions. 

I call this the 2280, because my exploration is a speculative historical insight into what could of evolved if the IC fabrication did to evolve to the point where they could put 5000 MOS transistors on a silicon die by 1971.

CDC chose not to use the Intel 8008, and evolved their own TTL processors for each generation of their product. They continued to use 2200 instruction set compatible boards up until the mid-1980s.


Version 1 was a bit-serial architecture, which used recirculating MOS shift register memory, and had a cycle time of up to 520uS. The 16-bit, 8 level stack was implemented using the first of the Intel NMOS SRAMs - the 3101.

Version 2 used a parallel architecture, and used dynamic RAMs with 500nS access time.  It was about 5 times faster for non-memory referenced instructions, and up to 100 times faster for memory operations.

It was the slow, and variable access time of the recirculating MOS shift-register RAM, which led to the Version 1 being designed with a rich register set. Frequently required data could be placed in a register rather than having to access the MOS SR-RAM.

Both versions are covered in the 2200 Reference manual:

https://github.com/Bread80/Datapoint2200/blob/main/2200_Reference_Manual.pdf

It was this Version 1 processor which would later form the basis of the Intel 8008, instruction set and ultimately the 8080 and Z80.

Fortunately a lot of documentation of this 8-bit, bit serial CPU remains, and much of it has been recreated using KiCAD and modern replica pcbs, in this excellent repository.

https://github.com/Bread80/Datapoint2200/tree/main

The processor is entirely TTL and consists of 117 individual ICs on a 15"x10" 2 sided pcb.

All of the ICs were selected from the Texas Instruments catalogue of 1969. As such there are a lot of basic gates, flipflops, multiplexers, 8-bit shift registers, counters and 64-bit bipolar SRAM used for the on-board stack.

93 of the ICs are in 14 pin DIL and the remaining 24 are in 16-pin DIL packages. So there are no big fancy chips.

The basic machine has an estimated transistor count of 2008, and the SRAMs used for the 16-bit x 8 level stack will use approximately a further 800.

This is a very low transistor count, for an 8-bt cpu, which offers the familiar A, B, C, D, E, F H, L, and (HL) registers.

An 8-bit shift register, such as the 74xx164, used 9 times on this design, will account for 450 of the transistor count.

The Program Counter based on 4 off 74xx193 4-bit up/down counters, uses a further 336 transistors, but this could be reduced considerably if more common 74xx163 counters were used instead.

The Github repo, of the recreated machine gives sufficient information for you to actually understand and build one of these processors.

Tri-state output devices were not available in 1969, so much of the bus-access logic is open-collector gates. This could be simplified, as could a lot of the other simple gate glue logic.

This would all help to bring the IC count (and transistor count) down.


There are a few points I would like to investigate:

1. Can I reduce the hardware in the program counter by making it a 16-bit shift register, with half adder incrementer, and single bit wide data paths from the other data sources. At the expense of increased instruction execution time, it would reduce the transistor count by around 200. (10%)

2. 8-bit architectures are non-optimal for a bit-serial architectures, because of the overhead of the ALU, clock sequencer, memory access hardware etc. More relative savings in hardware would be had from 16-bit register transfers and arithmetic/logic operations.

3. Based on point 2 above, extending the architecture of the Datapoint 2200 (Intel 8008) to include the 16-bit operations of the 8080 or Z80 would greatly improve the ratio of transistors used in sequential logic (registers, counters etc) to that used in combinational logic (multiplexers, glue etc).
The 8 level 16-bit stack in the Datapoint 2200 benefited in 1969 from bipolar static RAM like the 74xx89. These parts are virtually obsolete these days. We need a smarter way to make small register files from the parts we can still buy off the shelf from mainstream supplies.
