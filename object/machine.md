# Object Machine

The state of the object machine is completely defined by objects, in the same way a the state
of a c machine is defined by its memory. We will see below what objects, and how they are created.
We do use classes in the design, but a program running on the machine may work with just objects
in a sort of javascript way if it wants to.

![Vm Architecture](../diagrams/architecture.png)

Above is a basic diagram of the machine architecture. The following subchapters will detail many of
the aspects shown in more detail.

But the main thing to understand is that a dynamic object machine requires a relatively large
run-time. In an object oriented system, this means both data (objects) and functionality (functions).
And the easiest way to achieve the generation of the required
objects is to reuse the run-time at compile-time. This will be explained in the first sub-chapter.

In the following chapter we define our class system and how we achieve the desired dynamic behavior.
Then we will define the methods are represented and called. This will lead to the objects the
machine actually works on, mostly Messages.

Then we delve into the aspect of code generation and the layered structure to achieve this in
small definable Passes. Finally we represent the full instruction set for completeness.


### Language independent

We define the machine language independent, and aim to be general in a way as to allow many,
or at least several major, languages to be implemented using it. But for the details,
the implementation is needed, and off course any implementation needs to be in some language,
as to be understandable by humans.

The first implementation language will be ruby, or rather a subset thereof.
But to achieve language independence, a language independent file format is included.
That means that all objects that the machine is made up of, may be saved in a language
independent format and thus interchanged between languages. The format we use is text based,
not unlike yaml, yet in a much condensed form to make it easier to read. While this is an
important part of the machine, it is mostly technical detail and we refer the reader to
the details [here](../appendix/sof.html).
