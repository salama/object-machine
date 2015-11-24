## Syatem and Language

To define the object system, we need to define what it should be able to do: The purpose of the
system is to run object oriented programs, and so we need to talk a bit about what those are.

### OO language features

Since Smalltalk, many object oriented languages have been defined and implemented. These differ
in syntax, how dynamic they are, and which oo features they implement. Especially how dynamic
they are has determined if they are compiled or interpreted.

Our goal is to be language independent in the sense that any dynamic object oriented language may be executed by the system.

#### Objects and classes

The machine must off course support the idea of objects with attributes and methods.

Also each object has a class that may not change during it's life-cycle. A class may derive from
other classes and derivation may change. 

An object may also carry methods directly and this allows some definition of meta or eigenclass.

#### Message passing

Object oriented message passing must be possible. Message passing, as opposed to function calling,
resolves the function to be called at run-time.

This does not exclude function calling, which indeed must be supported too. Not only for cases
when the receiver is fully determined, as for example in class methods, but also to be able to
implement message passing.

#### Fully dynamic

The goal is that almost all aspects of the system are available to be changed at run-time. Some very basic aspects may require recompilation.

#### Exceptions

Exceptions are an alternate return path over potentially many method invocations. We support
raising and catching exceptions, ensure and retry.

### Object System

A system capable of running programs defined by an object oriented language as sketched above,
can be, and has been, implemented in many ways.

Our main requirement is that the system be what is called object oriented in languages.

#### Object based

Instead of calling it object oriented, the system is really object based, ie based on objects,
not oriented towards them.

Our aim is to define a set of objects, and a way of translating them to hardware instructions, that can implement an object oriented language as defined above.


#### Explicit values

We make explicit distinction between objects and values, which has often tried to be blurred.

All data is ultimately stored in values. Values are immutable and have no class. Values have type,
and we design an extensible type system. We define the minimal set of integer,reference and boolean
types, and note that especially float types demand external run-time type information (rtti).
