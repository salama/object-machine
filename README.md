# The object machine

**Design and Implementation**

In this book we set out to define the concepts of an object machine.
We call an Object machine a system that is able to run dynamic object oriented languages.
As such it is like virtual machines, but it is not interpreted.
Rather we aim for  a system that is completely defined by objects and their api.
It's more of a language, but without the syntax.

First we define the design goals, and the design, and go on to detail an existing example
implementation for the most common cpu, again in object oriented terms.

### Intended audience

This is not a scientific book, rather written by an enthusiast. As such it hopes to appeal to
other interested people, so if you like

- compilers
- how to build languages
- how oo is implemented
- virtual machines
- an overview of salama project

this should be interesting. Also this is an open source book, so if you find something that is
unclear or could be improved, file an
[issue in github](https://github.com/dancinglightning/object-machine/issues).

### Outline

The book starts by outlining the subject, also in context of previous work and by
defining design goals for the machine.

The second part is about how the system is implemented, specifically this is by defining it's own
object oriented system language, and mapping higher languages to that.

The next chapter is about this lower level system language.

Then we go on to translate the system language to an abstraction of current hardware, the register machine.

And the last part is about a concrete implementation on a version of the most common cpu today,
the arm.

### Status

The project that is the basis for this book is open source and [online](https://github.com/salama/salama). It is also in progress. New insights and a better
understanding of a more complete implementation lead to better design which in turn means
this book still changes too. Feedback for both book and project are welcome though.
