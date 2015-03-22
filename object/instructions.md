## Instruction Set

The previous chapter, specifially the diadram, showed how the Method holds Blocks which in turn holds Instructions. Some of the Instruction classes were brushed on to explain the class structure. This chapter provides detailed explanation of all Instructions defined by the machine.

In the next chapter we will start to define the mechanism of how the oo instructions are translated to executable code. This will be done in an extensible manner, and since the Instructions are defined as Subclasses of a Baseclass, additional Instructions may be defined by external code. In this way the machiene Instruction set is extensible, and may be executed by the machine implementation as long as mappings to lower levels are provided.

This extensible instruction set is quite different from physical machines, but also from other defined virtual machines.
Another marked difference is the fact that Instructions, as objects, may hold references to other machine objects. This is mostly a consequence of the the fact that we didn't start with a textual definition of the instruction representation. Also assembler instructions often may have references to other data, but this is often in a quite cryptic way, due to the way they have been designed as text representation. As mentioned we do have a text representation, which is human readable, but that is not it's main purpose. It's main purpose it to allow languages to exchange data in a text format that provides diffs, easy parsing and some readbility.

![Instruction Diagram](http://yuml.me/fc82f1db)


### Set

The possibly most used instruction is Set, which moves data from one object to another. Since we have an object machine, there is no other place for data to be but inside objects. This is a bit like the opposite of a register machine model, which never moves data straight from memory to memory.

The Set Intruction needs the two addresses of the data, and to model that we use an interger index into the object. So the machine views the object as an array-like structure, with the fixed overhead of type and layout described earlier. While the index is 0 based, that includes the Layout and as such any payload data starts at index 1.

While it would have been nice to design the access by name, this requires non-existing hardware support, like content addressable memory, to be efficient. On current hardware it would fix the implementation of the inevitable name to index lookup, which we wanted to avoid. There is an instruction for that lookup called InstanceGet, see below.

The Slot unifies the object/index pair into a sort of address and the Set takes to and from addresses, and moves data between them. For two objects a and b with instance variables foo and bar, meta code for the Set instruction would be

```
    a.foo = b.bar
```

Rembering that the machine does not have arbitrary object access, but that just 4 objects are accesible, message, frame, self and new_message.

### InstanceGet

As we have seen the Set is the way to move data between objects. The address of a value to be moved is a Slot instance and for many known variables constant objects exist to refer to them. But for the self object, the program must be able to access instance variables by name, and this what an InstanceGet does.

The result of InstanceGet is a Slot though, which can then be used in a Set instruction.

InstanceGet is one of the few instructions whose implementation at the object machine level, results in a Set, ie another object machine instruction.

### MethodEnter

Method enter is the first instruction of any function and basically gives the machine the oppertunity to do whatever needs to be done before any actual work may commence. In a register based implementations this would include saving the return address and possibly other register shuffling.

Another resposibility of this instruction is to determine whether a Frame will be needed. A seperate instruction exists to create a Frame,as we will see below. MethodEnter retains a reference to the Method that is entered to make the decision about the Frame.

### NewFrame

NewFrame is an instruction to create a new Frame object. When a method executes it stores (see Set) temporary and local variables in the Frame. In the implementation the Space keeps a pool of Frame objects which can be handed out quickly.

### MethodCall

A MethodCall instruction issues the call to a specific Method. The Method is not resolved, but must be given as a parameter. The implementation resolves the memory address and jumps there. 
It does other things which we will explain later.

### MethodReturn

MethodReturn is used to return from a Method. There is no argument, like the return value, as the return just restores the state of the machine before a call. The return value is a variable of the Message object and if the caller is interested it can use it from there (via Set).

The return address and previous Message is stored in the current Message so it is quite easy for the register implementation to shuffle the data around and jump back.

### MessageSend

MessageSend is the instruction that is used to represent the most common case in object oriented languages where a method and receiver are specified, but the actual Method needs to be resolved. The normal resolve is to check for a Method in the receivers object (ie it's layout), and then traverse the class hierarchy. Should none be found a method called method_missing is resolved instead, and this is guaranteed to be found in Object.

This implementation is off course quite complicated and as such implemented in code at the object machine level (not the register machine level). The details off this will be explained in the next chapter.

### NewMessage

The NewMessage instruction makes a new Message available for a Method. For preperation of the next message send, the new message is one of the four objects that can be accessed. Making a call will then swap the new message as the current message.

Messages, like Frames, are kept as a linked list by the object space. This linked list of messages has the function of the stackin a register machine. But since Messages are objects, they are easier understood and handled.

### Branches

#### Branch

Branch is a an abstract base class and as such instances have no meaning. All branch instruction nevertheless must derive from this class and have meaningful names that describe what it is they branch on.

The base class carries the branch target, ie the block that the branch will jump to if positive. If the branch is not taken, control flows implicitly to the next Block. 

Notice that a branch must be the last instruction in a Block, and also that the target must always be a Block, not an Instruction.

### UnconditionalBranch

An UnconditionalBranch always branches. So control never flows implicitly beyond it. The next Block must then be reached by another Branch.

UnconditionalBranches are for example needed in while loops that evaluate end criteria at the beginning, not the end, of the loop.

### IsTrueBranch

IsTrue branches when the object is true, and true means neither false not nil. The object that is being evaluated is the return value. This may sound strange at first, but return doubles as the last evaluated expression value.


