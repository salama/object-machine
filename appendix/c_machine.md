## C Machine

## Introduction

C has off course dominated software development, especially in the tool sector, for decades.
This is because it is a good and straightforward, simple, abstraction. In the following discussion
we do not want to make a general point about c being bad in any way. It is only a discussion
about c's suitability for writing a pure object oriented system as discussed in the beginning of the
book.

Also c, like all higher languages, has been created to be able to write higher level code and
not have to deal with the complexities of the underlying hardware. In this it works well, but off
course there are choices that were made and not all of them are obvious when one doesn't know
assembler.

## Supporting tools

One of the things that are not obviously clear when one talks about using the c language, is that
by doing so one buys into the complete tool chain that was created for the c language. These tools
are not just the c compiler as one might expect, but also static and or dynamic linker.

Off course, one might say, but it is still worth mentioning, as object files used by these tools
off course lean towards c based languages. And while they have been extended towards c based
static object orientation, they are quite not-object oriented.

## Assembler roots

One can look at c as a way to write higher level assembler. As a way not to have to write the
assembler instruction, but still effect a very defined outcome. In fact that is most likely what
was intended in the beginning. Off course as the language grew in features, the mappings to assembler
became less and less clear and with optimization techniques and so may cpu's it is unlikely that
anyone but a compiler writer can predict the mapping to assembly anymore.

Still, the point is that c is perceived as a language that can map to assembler in a straightforward
manner and that the concepts of assembler are basically available in C.

## Missing features

The above assumption is not true. Maybe it was at the time of c's creation, but it is not anymore.
Off course not, as many cpu's have hundreds of specialized instructions, especially for dealing
with multimedia nowadays, but also operating system functions. The c is and was to write inline
assembly for these cases.

But not all of these cases are *excotic* and we just want to mention a few that are not well known,
but potentially very useful.

### Overflow

C lets us branch on condition in if's or while's etc. These condition are are often modeled around
the zero or sign flags of mot cpu's status registers. Ie when you write a > b, the compiler
subtracts a from b and checks whether the result is positive by checking the flag. Or for a == b
it subtracts and checks if the zero flag is set.

Many cpu's will have an overflow flag too. This gets set when you add a + b, but the result
does not actually fit into a machine integer anymore. C has no branch possibility for this.
In fact it is difficult to even check this, and even more difficult when you go to multiplication.

So branch on overflow is something that c has no language equivalent for. Off course one could argue
that for this case one can build a function that uses assembly, but the point here was that the
language does not represent what a cpu can do.

### Compare and Swap

Compare and swap is an operation that takes an address and two values. It compares the value at the
address with the first value and if the two are the same, it changes it to the second value given.

Compare and swap instructions are found in most modern cpu's as single instructions because they
are the basis for lock free concurrent programming. Again C has no concept for this very important
construct.

### Pinned registers

The way that c works when calling functions means that it is not possible to use registers in a
predefined way. The idea is off course that you shouldn't have too, and for c programs you don't.
But when implementing a new language it may be helpful to do things differently, and the point is
that we couldn't.

C uses pinned registers just like the cpu does. Every cpu will have a Program Counter (pc) to
know which instruction to execute next. And C invariably uses a stack register to handle
argument passing. In the same way that the pc is used by the cpu, the stack is used by the
compiler, and in all likelihood created for the c language.

This is just to show the usefulness of pinned registers in language implementation and the main
point here is that c does not allow us to define more pinned registers.

### Multiple returns

A c function always returns to one point, the caller. And while you may nod at this and think off
course, it is still a restriction. In fact working without this restriction makes exception
handling much easier, as a the exception just returns to a different caller.

Also multiple returns make a different approach to the type system possible, where functions return
to different addresses, based on the type that they return. This makes it possible that the code
that is "returned" to is certain of the type of value it receives. The quotes around the return
mean to indicate that because of the multiple return addresses, the function may return to a
different place than where it was called from, and so the word return is meant more in a semantic
rather than literal way.

## Hidden features

A C compiler implements what we call the c-machine. Ie it adds all that hidden functionality that
makes the code actually work in the expected way. Primary among those are function calling and the
argument passing involved.

C does not define a way to access arguments or local variables from another thread. So any stack
access needs internal knowledge of the compiler. Stack walking is necessary for implementing
exceptions and garbage collection.

## OS interface

C has the standard library which defines a set of standardized functionality, much of which is
actually provided by the operating system. This access of the operating system in a functional and
defined way  was most likely the motivation behind the std lib.

To implement any useful language or virtual machine, one does off course need the operating system
support. But this does not necessarily have to be done through the c library.

As application programmer one does not necessarily think about it, but off course one can not
access system functionality through function calls. If one were to, that would mean one could run
arbitrary code inside the os. Instead, operating system access is achieved with system interupts,
signalling to the os the required code. The os suspends the program and executes it's own code,
usually after switching cpu mode and address space.

But the point here is that one can not call the operating system. Also there is none of the
c complexity off argument passing. Argument order and meaning are defined by the os, not c, and may
be easily implemented by any tool that can emit binary code.

### c lib

As a side note to above point, c libraries have grown large, both in terms of functionality and in
size of binary. As much of the library defines basic (non-os) functionality that can be coded in any
language, it's usefulness for another language decreases even more.

In fact, as can bee seen by ruby's std lib, including c-lib functionality into a new language
may just lead to duplication and an old convoluted api.

## Static vs dynamic

C code was meant to be (and usually is) compiled into executables that do not change. A c program
is fully defined at compile time and does not change, let alone itself, at run-time.

This static nature is what makes it most incompatible with the kind of oo machine we have defined.
