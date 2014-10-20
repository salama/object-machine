# The object machine

**Design and Implementation**

In this book we set out to define the concept of an object machine, which is related to, but not the same as, a virtual machine.

First we define the design goals, and the design, and go on to detail an existing example implementation for the most common cpu.

### Intended audience

This is not a scientific book, rather written by an enthusiast. As such it hopes to apeal to other interested people, like if you are interested in

- compilers
- how to build languages
- how oo is implemented
- virtual machines
- an overview of salama project

this should be interesing. Also this is an open source book, so if you find something that is unclear or could be improved, file an [issue in github](https://github.com/dancinglightning/object-machine/issues).

### Outline

The book starts by outlining the subject, also in context of previous work and defining design goals for the machine.
The second part is about the actual object machine, it's parts and instructions. Then we go on to map this to an abstraction of current hardware, the register machine.
And the last part is about a concrete implementation on a version of the most common cpu today, the arm.

### Status

The project that is the basis for this book is open source and [online](https://github.com/salama/salama). It is also in progress. New insights and a better understanding of a more complete implementation lead to better design which in turn means this book still changes too. Feedback for both book and projet are welcome though.

