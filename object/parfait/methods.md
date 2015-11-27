## Methods

As we have seen in the previous chapter, that the object machines objects contain,
as their instance variables, binary data and methods. Off course, in an object oriented system,
the distinction is not so great, as methods are represented as objects too., but still, only
methods are callable.

From a application programers perspective the Method object is quite opaque and it is mostly
the name that is interesting. Methods are callable and that is what matters.
But for the machine the structure is essential to the working of the machine.
And how methods actually provide the callable functionality is the topic of the rest of this chapter.
The diagram below gives an overview of the classes involved.

![Method diagram](../diagrams/control.png)

### Source and Binary

The method keeps keeps it's source in two forms. The parsed ast is kept, mostly for later inining.
And the generated stream of instructions is later used to assemble a binary form. This is stored
as the Code.


### Labels and instructions

In the object machine Code is represented as a linked list of instructions.
The exact definition of the Instructions will be a subsequent chapter,
here we just want to explain the parts of the hierarchy that pertains to flow control,
as this influences the structure of the Method.

We can see from the diagram that the Call instruction holds a reference to a Label,
the start Label of the method that is to be executed when the instruction is executed.
But to implement control structures we also need a jump or Branch instruction.
Branches have been banned from programming languages, where they are often called goto,
for a good reason, namely they are so low level as to be difficult to understand.
Instead languages use higher level control structures. But to implement these control structures,
we do need a Branch, and so we also need a place for a branch to branch too.
As in assembler we call these places labels.

### Calling

Quite apart from the details of how we generate instructions and their structure,
above class design lets us clearly implement control structures and function calling.
But it does not explain how a function call would actually happen, just how the instruction is defined.

To understand function calling, let us look very briefly at how this is done at the cpu level,
and what the c machine does. While branching just transfers control,
calling transfers control with the intent to resume after the call.
That means the address where control resumes is needed.
Many cpus push the return address onto a system stack, but some, like arm, store it in a designated register.
This shows how the return is really a hidden argument to the function,
much in the same way that the object (self) is  a hidden parameter in object oriented languages.

The c machine, implemented by the compiler, uses registers, the stack or a combination of them,
depending on cpu, to pass function arguments. If a function uses local variables,
these are usually stored on the stack, and the area designated for this is called the frame.
All this moving about of data to and from registers and the stack is what lets the programmer
deal in the language concepts, essentially is the implementation of the c machine.

Off course we need to pass objects in the object machine too, and it is quite straightforward
to define an array for this. The question is more how to pass the implicit information,
since we want to do this too in an object oriented fashion.

#### Message

Because the function must often be resolved at run-time, one often speaks of sending messages
in object oriented languages. So to make all information passed explicit, in an oo fashion,
we define a Message class with the following attributes:

- Name, the name of the function to be called on the *receiver*
- *receiver* , the object receiving the message
- arguments, an array object containing the arguments in order. Arguments are typed values as described above.
- return addresses (per type and exceptional)
- Frame object for local and temporary variables

A Message is the basis for message sending that is the basis of object oriented programming.
Sending a message involves finding the right Method and calling that. So all sending boils down to
calling a method, which is what happens when an object "receives" a message.

#### Calling

Calling a Method is relatively simple: We acquire a message (see below) and fill in the data,
arguments, method name and receiver. Then we have to match the arguments types against to find
the right Code to call.

In fact, call is just a name for a branch that is meant to return. Soml is quite low level, and
gives the programmer control over where to return to. So the caller, not the callee (as in stack
based languages) determines the point of return. This feature is the basis of later concurrency
features.


### Static Message Chain

An interesting and not immediately obvious consequence of this design is the ease with which it is
implemented. Specifically all messages may be created at compile-time and linked into a double linked
list. The forward link lets us easily make a new message available, and the backward link let's us
return the previous state.

Especially programmers who may think of call graphs, may wonder why no dynamic, or run-time, work is
required here. Why a list is enough and not a graph needed.
Maybe the easiest way to understand this is by analogy to the c world. C uses a stack, ie an array,
not a linked list. But the important thing is that the stack is also allocated at compile-time and
does not change. Or it may have to be extended when space runs out, but that is not covered in this
discussion (neither for the messages).

Making a new message available is thus really easy and that does off course mean fast. All we need
to do is follow the link of the list, so in essences one cpu instruction.

### Exceptions

As we have seen, the return address of a regular return is a hidden argument in the Message.
To implement exceptions we need to be able to "return" to a point across several function
invocations, but in essence it is the same thing. We store the last exception address in the Message.
This may be changed and setting it is equivalent to the language concept catch that many languages have.
If it is not set in a Method, a raise will traverse the Message chain until it finds
a Message where it is set and return to that point.

A side effect of this object oriented design is the ease with which such a raise/catch may be
implemented. We do not need to unwind any cryptic stack frames or free any memory or indeed
keep book of anything to be freed or cleaned up. Only the Message data, especially arguments
needs to be zeroed for garbage collection to work properly.

## Machines Objects

The machine, being object oriented, must work on objects. In fact only one object is enough, and
that is the current message. Everything needed is available from the message, with the possible help of
globals. As usual in oo Systems, class objects (the objects referring to a named Class instance)
is available by writing the class name as capital.

And by available we mean that soml language features like object attribute getting/setting, calling
methods etc (whilch then compile to register machine intructions) is all that is required to define
the machines working.
