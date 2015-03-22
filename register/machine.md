# Register Machine

As the previous chapter outlines, the Register Machine is a register based virtual machine that abstacts current hardware and sits between the object and arm machines. As a machine it is it's own virtual machine, though we are only interested in how the object machine is implemented on it. It is in a way an abstraction not unlike the c level, but much less complete.

The name Register does not only refer to the fact that the machine uses registers, but also that all work is done in registers. Off course Regsiter machine uses memory (not objects) and so can load data from memory and store data back to it. Another defining characteristic is that it can not transfer data from memory to memory, as the diagram below illustrates

|  From/To| Mem | Reg |
| -- | -- | -- |
| Mem | NOT ALLOED | LOAD |
| Reg | STORE | TRANSFER |

Besides this, the register machine is defined by the instruction set, which we will define in the second subchapter.

But this is not an abstract machine defined for general purose. We want to map the object machine to it, so the first thing we will need to detail is how the registers are used. This is the first subchapter.

As mentioned elsewhere, one reason we define this level at all is to make implementation on other hardware easier. But even, or mainly, it provides an understandable model with easily understandable names, for this level.

The Register machine has no explicit runtime type information. It does have types, namely signed and unsigned integers, but all information about these is implicit in the code (like c). Signed and unsigned int correspond to integer and reference types of the oo machine.