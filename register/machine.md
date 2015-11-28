# Register Machine

As mentioned, the Register Machine is a register based virtual machine that
abstracts risc hardware and sits between soml and arm machines. As a machine it is it's
own virtual machine, though we are only interested in how soml is implemented on it.
It is in a way an abstraction not unlike the c level, but much less complete.

The name Register does not only refer to the fact that the machine uses registers,
but also that all work is done in registers. Off course the Register machine uses memory
and so can load data from memory and store data back to it, but it is a defining characteristic
that it can not transfer data from memory to memory, as the diagram below illustrates

|  From/To| Mem | Reg |
| ------- | ----- | ----- |
| Mem | NOT ALLOED | LOAD |
| Reg | STORE | TRANSFER |

Besides this, the register machine is defined by the instruction set,
which we will define in the second subchapter.

But this is not an abstract machine defined for general purose.
We want to map the object oriented concepts to it, so the first thing we will need to detail
is how the registers are used (below)

As mentioned elsewhere, one reason we define this level at all is to make implementation
on other hardware easier. But even, or mainly, it provides an understandable model with
easily understandable names, for this level.

The Register machine has no explicit runtime type information. It does have types, namely
signed and unsigned integers, but all information about these is implicit in the code (like c).
Signed and unsigned int correspond to integer and reference types of the oo machine.

## Register use

This chapter defines how we will used the registers of the Register Machine to
implement Soml. Specifically calling and the difference between statements and expressions

We will see that the mapping we define does rely on the machines ability of indexed memory access.
We shall define that in detail in the following instruction set chapter,
but briefly it allows us to store a base memory address, the pointer to or reference of an object,
and access memory at an indexed offset to that register with a single instruction. Get/Set Slot and
Byte rely on this.

### Calling

Method calling is implememted by "sending" messages. The quotes mean that at the soml level actual
sending does not realy happen, more like a handing over. Still, the Message defines all
information needed to execute a method, and return from it.

**The current Message object is always in register 0**

### Statements

Soml statements have full use of the register. In other words, each statement can use all registers
in whichever way it sees fit. Between statements, all information must be stored in objects.

** Each Statement may use all registers **

The Message carries arguments and a frame for local variables, so by the end of the statement
temporary values should be stored in the variables they represent.

This is the current situation, but optimisations are planned for three objects, the receiver,
arguments and frame. By the current model these need to be loaded often, and keeping track
of these loads at a function level (in the compiler) could improve performance noticably.

### Expressions

** An Expression results in a loaded register**

Somls expressions are designed to resolve to a value. Compiling an expression results in a register
which holds that value. It may take one or many instructions to achieve this. Expressions may even
use temporary registers to achieve their result. But in the end, all expressions resolve to a
register which is then usually used by a statement.
