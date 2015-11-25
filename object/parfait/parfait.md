## Parfait, the run-time

A dynamic object machine allows the programmer to access a much of the machine at run-time.
Even for a minimal system this requires Classes, Methods basic numeric integers and with
supporting classes we come to the diagram below.

![Vm Architecture](../diagrams/parfait.png)

The diagram leaves out all detail of associations and functionality for clarity. We will go into
all the aspects as needed in further chapters.

Some basic classes, List, Dictionary, Word, Interger and Float are just what you would expect and
won't be covered. The only exception here is that Integer and Float are not modeled as objects as
in other systems, but rather as an even more base class called Value, which we will go into in
the next chapter.

### Parfait - Adapter

For the purpose of creating the machine we will need to create an initial state of the machine.
As we only have objects, this is a collection of objects, which is traditionally called the
ObjectSpace (or just Space). In it's serialized form, Smalltalk described it as an *image*.

Obviously the easiest way to build such an image is to use the same classes as the image
represents. Otherwise one is just duplicating the code, as all class names and associations are in
fact the same. But while this is an obvious direction, it is not so clear how to do this.

Since the language we implement the machine in has in essence the same functionality as our
desired object machine, we can code and use much of the Run-time and "just" use it at compile time.
This is off course not completely true, as the system must include object creation and this is
handled by the machine that is running the compiler, ie ruby. But for the most part we do not need
the exact memory layout, it is enough the objects respond with the right data for the associations
they have. Only for List (and derived classes) do we need to *fake* the memory where the data is
stored.

So in summary, by using a small (less than 100 lines) adapter, we are able to use the Parfait
run-time classes at compile time and build the *image* as we need it. To create an executable
we create a binary representation.
