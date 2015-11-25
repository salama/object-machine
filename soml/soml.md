---
layout: soml
title: Salama object machine language
---


#### Top down designed language

Soml is a language that is designed to be compiled into, rather than written, like
other languages. It is the base for a higher system,
designed for the needs to compile ruby. It is not an endeavour to abstract from a
lower level, like other system languages, namely off course c.<br/>
Still it is a system language, or an object machine language, so almost as low level a
language as possible. Only assembler is really lower, and it could be argued that assembler
is not really a language, rather a data format for expressing binary code. <br/>

##### Object oriented to the core, including calling convention

Soml is completely object oriented and strongly typed. For types, the classes are used, but
the main distinction is between object (references) and integers. This is off course
essential as dereferencing integers is what we want to avoid.

The object model, ie the basic properties of objects that the system relies on, is quite simple
and explained in the runtime section. It involves a single reference per object. <br/>
Also the object memory
model is kept quite simple in that objects are always small multiples of the cache size of the
hardware machine. We use object encapsulation to build up larger looking objects from these
basic blocks.

The calling convention is also object oriented, not stack based*. Message objects used to
define the data needed for invocation. They carry arguments, a frame and return addresses.
In Soml return addresses are pre-calculated and determined by the caller, and yes, there
are several. In fact there is one return address per masic type, plus one for exception.
A method invocation may thus be made to return to an entirely different location than the
caller.
\*(A stack, as used in c, is not typed and as such a source of problems)

There is no non- object based memory in soml. The only global constants are instances of
classes that can be accessed by writing the class name in soml source.

##### Syntax and runtime

Soml syntax is a mix between ruby and c. I is like ruby in the sense that semicolons and even
newlines are not neccessary unless they are. It still uses braces, but that will probably
be changed. <br/>
But off course it is typed, so in argument or variable definitions the type must be specified
like in c. Types are classes, but int may be used for brevity instead of Integer. Return
types are also declared, though more for statci analysis. As mentioned any function may return
to differernt addresses according to type. The compiler automatically inserts erros for
return typesa that are not handled by the caller. <br/>
The complete syntax and their translation is discussed <a href="syntax.html"> here </a>

As soml is the base for dynamic languages, all compile information is recorded in the runtime.
All inforamtion is off course object oriented, ie in the form off objects. This means a class
hierachy and this itself is off course part of the runtime. The runtime, Parfait, is kept
to a minnimum, currently around 15 classes, described in detail <a href="parfait.html">
here </a>. <br/>
Historically Parfait has been coded in ruby, as it was first needed in the compiler.
This had the additional benefit of providing solid test cases for the functionality.
Currently the process is to recode the same functionality in soml, and by the end of that
a converter will be written. This will convert the soml code into ruby code, thus removing the
duplication.
