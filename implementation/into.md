# Implementation

We create an executable by translating the code from the register machine and combining the code
and binary representations of the objects in one file. Our test machine is running linux as the operating
systems which uses elf as it's binary format. As we don't want to get too dependent, the elf
encoding is kept very minimal.


## Arm instructions

The arm architecture, or versions of the arm architecture, are the most common cpu today.
This is in part due to the low power consumption, which may in turn be attributed to the simple
design and low transistor count. A basic arm6 processor has 30 thousand transistors, while a
modern intel has over 3 billion (not million).

This is also reflected in the instruction set, which is quite small and also quite well structured.
Arm, being a risc architecture, has fixed length instructions, with a rigid encoding scheme.

Consequently six classes, implement most of the arm assembly (used outside os's), which is so well
documented elsewhere, we will not go into detail here.

The instructions used are listed below and do off course not even make up the larger part of the
whole instruction set. But enough to implement the object machine.

- MoveInstruction (mov, mvn
- StackInstruction (push, pop)
- MemoryInstruction (strb, str , ldrb, ldr)
- LogicInstruction (adc, add, and, bic, eor, orr, rsb, rsc, sbc, sub)
- CompareInstruction (cmn, cmp, teq, tst)
- CallInstruction (b, call , swi)

Arm uses conditional codes on *every* instruction and while this is very useful for very small
branches, it is one of the obvious optimisations that is not implemented.
