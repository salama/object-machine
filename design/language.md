## Machine and Language

To define the object machine, we need to define what makes a machine. And since the purpose of the machine is to run object oriented programs, we need to talk a bit about what those are. We start with the language as this is more familiar to software engineers.

### OO language features

Since Smalltalk, many object oriented languages have been defined and implemented. These differ in syntax, how dynamic they are, and which oo features they implement. Especially how dynamic they are has determined if they are compiled or interpreted.

Our goal is to be language independent, this is why we design a machine and not a language. Still, we would like our machine to be able to run as many languages as possible, so we need to make the desired features explicit. This we do with an eye to what can be implemented on top of another. If we take object encapsulation as an example: it is impossible to implement encapsulation when the machine allows unrestricted object access, but trivial to implement free access on top of encapsulation via hidden accessors.

#### Objects and classes

The machine must off course support the idea of objects, and as outlined above, we choose full data hiding. This means only the objects methods have access to its internal data.

Also each object has a class that may not change during it's life-cycle. A class may derive from other classes and derivation may change. As in ruby, we call classes that can not be instantiated modules. Eigenclasses are not only supported but integral to the machine.

#### Message passing

Object oriented message passing must be possible. Message passing, as opposed to function calling, resolves the function to be called at run-time.

This does not exclude function calling, which indeed must be supported too. Not only for cases when the receiver is fully determined, as for example in class methods, but also to be able to implement message passing.

#### Fully dynamic

The goal is that all aspects of the machine are available to be changed at run-time.

This includes Methods and the state of the machine, in other words it supports closures. But also the class hierarchy in all it's aspects and the type system of basic values.

#### Exceptions

Exceptions are an alternate return path over potentially many method invocations. We support raising and catching exceptions, ensure and retry.

### Object Machine

A machine capable of running programs defined by an object oriented language as sketched above, can be, and has been, implemented in many ways.

Our main requirement is that the machine too be what is called object oriented in languages.

#### Object based

Instead of calling it object oriented, the machine is really object based, ie based on objects, not oriented towards them. And even based seems a little verbose as it is really made up of just objects, so we call it object machine.

Our aim is to define a set of objects, including a set of instructions, that can implement an object oriented language as defined above.

Defining the objects as pure data structures is somewhat hard to understand, we will define them in an object oriented language and then define a method to externalize them in a language independent manner. The language of choice is ruby, but it should be noted that parsing ruby is not part of the object machine functionality.

#### Instruction Set

The set of Instructions and their meaning defines what the machine will actually do. The instruction set is what would be assembler for a cpu. But since we are not building a physical machine there is no need to define a concise binary encoding either. We will see in the implementation section how to transform object machine instructions to cpu executable instructions.

Off course Instructions, like other classes, are objects. For the machine to have defined behaviour, we need to define what the instructions do in terms of the other objects of the machine.

#### Explicit values

We make explict distinction between objects and values, which has often tried to be blurred.

All data is ultimately stored in values. Values are immutable and have no class. Values have type, and we design an extensible type system. We define the minimal set of integer,reference and boolean types, and note that especially float types demand external run time type informations (rtti).
