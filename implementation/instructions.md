## Arm instructions

The arm architecture, or versions of the arm architecture, are the most common cpu today. This is in part due to the low power consumption, which may in turn be attributed to the simple design and low transistor count. A basic arm6 processor has 30 thousand transistors, while a modern intel has over 3 billion (not million).

This is also reflected in the instruction set, which is quite small and also quite well structured. Arm, being a risc architecture, has fixed length instructions, with a rigid encoding scheme.

Consequently six classes, implement most of the arm assembly (used outside os's), which is so well documented elsewhere, we will not go into detail here.

The instructions used are listed below and do off course not even make up the larger part of the whole instruction set. But enough to implement the object machine.

- MoveInstruction (mov, mvn
- StackInstruction (push, pop)
- MemoryInstruction (strb, str , ldrb, ldr)
- LogicInstruction (adc, add, and, bic, eor, orr, rsb, rsc, sbc, sub)
- CompareInstruction (cmn, cmp, teq, tst)
- CallInstruction (b, call , swi)

Arm uses conditional codes on *every* instruction and while this is very useful for very small branches, it is one of the obvious optimisations that is not implemented.

## Passes

But as mentioned the Passes architecture makes it trivial to add optimisations later, even by third parties.

Even new Instruction classes may be defined and used, by defining new Passes at the register level.

As the mapping of Register Machine to Arm is quite trivial, we will not go into the details of the passes. With the same naming convention as was used for the register passes we just give a quick list:

- CallImplementation implements FunctionCall
- ConstantImplementation implements LoadConstant
- GetImplementation implements GetSlot
- MainImplementation implements RegisterMain
- ReturnImplementation implements FunctionReturn
- SaveImplementation implements SaveReturn
- SetImplementation implements SetSlot
- TransferImplementation implements RegisterTransfer

