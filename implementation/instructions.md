## Arm instructions

The arm architecture, or versions of the arm architecture, are the most common cpu today. This is in part due to the low power consumption, which may in turn be attributed to the simple design and low transistor count. A typical arm processor has 60 thousand transistors, while a modern intel has over 3 billion.

This is also reflected in the instruciton set, which is quite small and also quite well structured. Arm, being a risc architecture, has fixed length instruction, with a very rigid encoding scheme.

Consequently six classes, implement most of the arm assembly, which is so well documented elswhere, we will not go into detail here.

The instructions used are listed below and do off course not even make up the larger part of the whole instruction set. But enough to implement the object machine.

- MoveInstruction (mov, mvn
- StackInstruction (push, pop)
- MemoryInstruction (strb, str , ldrb, ldr)
- LogicInstruction (adc, add, and, bic, eor, orr, rsb, rsc, sbc, sub)
- CompareInstruction (cmn, cmp, teq, tst)
- CallInstruction (b, call , swi)

Arm uses conditional codes on *every* instruction and while this is very useful for very small branches, it is one of the obvious optimisations that is not implemented.

But as mentioned the Passes architecture makes it trivial to add optimisations later, even by third parties.

Even new Instruction classes may be defined and used, by defining new Passes at the register level.
