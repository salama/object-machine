# Object Machine

The object machine is completely defined by objects, in the same way a the c machine is defined by its memory. And just as some memory is function code, some of the objects define methods, some instructions and so on.

We do use classes in the design, but a programm running on the machine may work with just objects in a sort of javascript way if it wants to.

We will define the basic class mechanism in the next chapter and then go on to define how methods are represented and called.

Then we will define the instruction set and how they affect which objects. At that point we will have a self sufficient Set of objects we call the Object Space and if we were hardware engineers we could go and bend silicon into shape.

Since we want to produce a software machine we will need to talk a bit about layout and then the final chapter will detail the way we commence compilation.

After the objects and the instruction set is defined, we can create a self referential representation of the machine.

#### Basics asumed

Every object oriented language needs basic objects of byte and integer arrays. We will assume their existence without much detail. Given the description they will be easy to conceive, but tricky to get right in practise.

Also, slightly more complex, an object to object mapping, called Hash in ruby tradition, will be assumed.

Concrete implementations of these are interesting to some off course, but as long as the very simple interface is clear, the details have no influnce on our design or and little on the implementation.
