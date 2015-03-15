## Previous work

While we hope to make a meaningful step in the art, this has by no means been the first effort, even with very or very very, similar goals.

### Mother smalltalk

Like no other work Smalltalk-80, and especially the "Blue book" Smalltalk-80: the languge and it's implentation has influenced the success of object oriented thinking. Smalltalk is very pure in it's object oriented aproach and tries to extend that as far as possible into the implementation aspect.

As much as possible that is in a C based implementation. The actual machine is an interpreter and as such can not be changed and only minnimally inspected, by the programmer.

Smalltalk introduced the method of value tagging in which the essential runtime destrinction between integer and reference is made by a bit of the value. Integers are subsequntly shifted while the bit is masked out for references.

Without any categorization attemps (outside this book) let us call the Smalltalk-80 vm a first generation object machine.

###  Jvm, Squeak

Java is quite rare among the object oriented languages in that it has a complete and rigorously defined instruction set. Alas this instruction set is biased, though not completely, towards java. This is shown by the fact that several other object oriented languages are implemented on top of jvm. The bias shows in the in the restrictions that the jvm imposes on those languages.

The jvm is also c based, but uses just-in-time compilation to compile often used methods to machine code, so as to avoid subsequent interpretation. Squeak, an open source smalltalk implementation, follows a simmilar aproach, though it is much more lightweight.

Let's call this aproach second generation, again just for the purposes of this chapter.

### Rubinius

Rubinius is a ruby implementation which is first to even attempt a complete object oriented implementation.
It's core is implemented in c++, but it exposes c++ objects as sort of meta-constant ruby objects. Those objects may receive normal methods and as such work, but their meta-programming capabilities are restricted to c++.

Rubinius also uses a jit. Let's call this generation 2.5 , as it is very close to a pure object implementation.

### 3'rd generation

In the above terminology we are defining (and implementing) a third generation object machine. This will be marked by being completely object based, not interpreted, and not using c for code generation.

### Virtual C machine

All above examples use existing c or c tools for machine code generation. These tools (compilers/jits) implement the c machine and as such above aproaches all sit on top of that c machine.

The virtual c machine is not, as may be believed, a real machine or even an abstraction of a real machine. It probably started out as an abstraction of a then existing machine. But real machines have moved on and c has been extended to almost any machine with a proessor. This is just to say that by now there is a fair bit of functionality that is hidden from the c programmer, especially calling conventions.

On the other side there is a fair bit of hardware functionality that the c machine does not expose to the programmer as language features. Branching on overflow condition is a good example of this.C relies on the assembler integration to do that, and as such this can neer be part of the language core.

So by using a c implementation (or a to c translation), one builds on the c machine, and thus never has complete control over the actual machine. The c compiler inserts code that cannot be changed.
