## Register use

This chapter defines how we will used the registers of the Register Machine to implement the Object Machine.
We assume 8 general purpose registers here which we name r1-r8 and a **p**rogram **c**ounter we call pc.

We will see that the mapping we define does rely on the machines ability of indexed memory access. We shall define that in detail in the following instruction set chapter, but briefly it allows us to store a base memory address, the pointer to or reference of an object, and access memory at an indexed offset to that register with a single instruction.

|  | Message| Self| Frame| NewMessage | Scratch |
| ---| --- | --- | --- | --- | --- |
| **Register:** | r0 | r1 | r2 | r3 | r4 - r7|

The diagram shows where the object references to the object machines 4 objects are located. Registers up from 4 are used by the machine as temporary storage to implement instructions and never stored. To keep the object machines logic they must never be used across a branch or call.

