## Layers and Passes

We have now defined the main parts of the machine as an independant machine, which can be viewed as an abstract or virtual machine. To implement this machine in software there are many more pieces needed which we shall define as seperate layers.

Layers let us seperate the different parts and help in the process of naming, that software engineering is.

### Layers

Specifically the three main Layers we define are given in the diagram below. The top layer is the object machine that we defined, below it the register machine and below that the arm imlementation. Now normally layers are designed to seperate, and especially with this design we would excpect the arm layer to be independent of the object machine layer and vice versa. And while this is true for the most part, we will also see a way in which is is not in the next section.

![Main Layers](http://yuml.me/59576e82)

#### Register Machine

The Register Machine is an abstract model of current hardware. It has registers, memory, a program counter and a stack just like you would expect. But of the four possibilities of moving data between registers and memory, it does not implement direct memory to memory transfer. Hence the name, so all data must first go from memory to registers to be moved back to memory, after possible processing.

As our goal is to implement the object machine for a specific cpu, we would not really need extra layers. We still define the register machine layer for two main reasons.

The first reason and most compelling is that this allows us to implement different cpus with relative ease. Most cpus are quite similar and it is always possible to have a straightforward sub-optimal first implementation that has optimisation passes on the resulting code.

The other reason for defining the layer is that when implementing the cpu specific layer, some kind of machine abstraction happens automatically as the step between the object and arm machines is just quite big. So by formalising the layer we gain above mentioned advantage.

#### Arm machine

The Arm architecture is a simple RISC machine. It is a register based architecture with 16 registers, and a well defined and easy to implement instruction set. The register machine defines almost all aspects of the machine apart from small details like the memnonic names or their encoding.

### Passes

Since we have now defined the layers we shall use, the process of generating an executable becomes tumbling down those layers. We need to define the Object Machine in terms of the Register Machine, and then the Register in terms of the Arm machine. But this is true for the instructions only, other objects of the object machine stay valid.

This and other reasons have prompted us to use an iterative aproach to the conversion process. We call these iterations Passes. A Pass runs, or passes, over the instruction stream of a Method and changes it as it sees fit.

The result of all Passes of a layer results in the desired transformation from one machine instruction set to another. In other words the sum of one machines Passes represent a compilation to that machine.

A simple example of a pass would be how the register machine implements the object machine Set instruction. While adherring to some register layout, the Set is replaced by two instructions, one load and one store. Another more trivial example is how the arm machine later transforms a load into ldr (the arm **l**oa**d** **r**egister instruction)

There are several advantages to using Passes, which are mainly related to the fact that the data structure we work on is "intact", though it may be partial. At any point in the process one can for example take a snapshot of the state in a human readable format (sof). Other advantages are :

- small steps. Each Pass usually only touches one instruction, possibly two.
- defined steps, sort of like a state machine
- much easier to understand
- easier to do half steps (or indeed makes it possible, see below)
- easy to extend at any point in the process

Passes are registered at object space level and can ordered before or after other known steps. So even external libraries can add Passes to any compilation step for any machine.

The observant reader will have noticed that there are some object machine instructions that resolve to other object machine instructions. So the object machine itself makes use of the Pass architecture and transforms its own MessageSend instruction to it's own instructions, including a MessageCall.

Both following chapters about the definition of the Register Machine and the Arm implementation rely heavily on this Pass architecture.

