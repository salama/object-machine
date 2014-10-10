# Machine and Language

To define the object machine, we need to define what makes a machine. And since the purpose of the machine is to run object oriented programs, we need to talk a bit about what those are. We start with the language as this is more familiar to software engineers.

## OO language features

Since Smalltalk, many object oriented language have been defined and implemented. These differ in syntax, how dynamic they are, and which oo features they implement. Especially how dynamic they are has determined if they are compiled or interpreted.

Our goal is to be language independant, this is why we design a machine and not a language. Still, we would like our machine to be able to run as many languages as possible, so we need to make the desired features explicit. This we do with an eye to what can be implemented on top of another. If we take object encapsulation as an example: it is impossible to implement encapsulation when the machine allows unrestricted object access, but trivial to implement free access on top of encapsulation via hidden accessors.

#### Objects and classes

The machine must off course support the idea of objects, and as outlined above, we choose full data hiding. This means only the object has access to its internal state (data).

Also each object has a class that may not change during it's lifecycle. A class may derive from other classes and derivation may change. As in ruby we call classes that can not be instantiated modules. Singleton classes are not only supported but integral to the machine.

#### Message passing

Object oriented message passing must be possible. Message passing, as opposed to function calling, resolves the function to be called at runtime.

This does not exclude function calling, which indeed must be supported too for cases when the receiver is fully determined, as for example in class methods.

#### Fully dynamic

The goal is that all aspects of the machine are available to be changed at run-time.

This includes Methods and the state of the machine, in other words it supports closures.

#### Exceptions

Exceptions are an alternate return path over potentially many method invocations. We support raising and catching exceptions, ensure and retry.

## Object Machine

A machine capable of running programs defined by an object oriented language as scetched above, can be and has been implemented in many ways.

Our main requirement is that the machine too be what is called object oriented in languages.

### Object based

Instead of calling it object oriented, the machine is really object based, ie based on objects, not oriented towards them. And even based seems a little verbose as it is really made up of just objects, so we call it object machine.

So we aim to define a set of objects, including a set of instructions, that can implement an object oriented language as defined above.

As defining the objects as pure data structures is somewhat against the working of the human brain, we will define them in an object oriented language and then define a method to externalise them in a language indepenedant manner. The language of choice is ruby, but it should be noted that parsing ruby is not part of the object machine functionality.

### Instruction Set

Even when defining the machine in a language, when externalised, it is pure data. There is no functionality in it.

So we need to define a Set of Instructions and their meaning to define what the machine will actually do. The instruciton set is what would be assembler for a cpu. But since we are not building a physical machine there is no need to define a concise binary encoding either. We will see in the implementation Section how to transform object machine instruction to cpu executable instrcutions.

Off course Instructions, like classes, are objects. For the machine to have defined behaviou, we need to define what the instructions do in terms of the other object of the machine.

### Explicit values

We make explict distinction between objects and values, which has often tried to be blurred.

All data is ultimately stored in values. Values are immutable and have no class. Values have type, and we design an extensible type system. We define the minimal set of integer and reference types, but note that especially float types demand external run time type informations (rtti).
