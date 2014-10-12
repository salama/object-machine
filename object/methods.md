## Methods

As we have seen in the previous chapter, the object machines objects contains, as their instance variables, bit data and methods. Off course, in an object oriented system, the destinction is not so great, as methods are represented as objects too.

From a programms perspective the Method object is quite opaque and it is mostly the name that is intersting. Methods are callable and that is what matters.
But for the machine the structure is essential to the working of the machine. And how methods actually provide the callable functionality is the topic of the rest of this chapter.
The diagram below gives an overview of the classes involved.

![Method diagram](http://yuml.me/23cef8f6)

### Blocks

The idea of Methods is that they are callable and execute code. In th eobject Machine Code is represented as a sequence of instructions. The exact definition of the Instrucitons will be a subsequent chapter, here we just want to explain the parts of the Hierachy that pertains to flow control, as this influences the structure of the Method.

We can see from the diagram that the Call instruction holds a reference to a method, obviously the method that is excuted when the instruction is executed. But to implement control structures we also need a jump or Branch instruction.
Branches have been banned from programming languages, where they often called goto, for a good reason, namely they are so low level as to be difficult to understand. Instead languages use higher level control structures. But to implement these control structures, we do need a Branch, and so we also need a place for a branch to branch too. In assembler these places are called labels.

If one looks at instruciton sequences from a control flow perspective they fall into little linear chunks, interspersed by calls and branches. We call such a linear sequance of instrucitons Block, and with that we can see that only Blocks are legal point to branch to.

So a Branch instruction holds a reference of the Block it jumps to. A Block in turn is a linear sequnce of Instructions and if it contains a Call or Branch instruction it does so as it's last instruction. Methods hold a sequance of Blocks, and if a Block does not call or branch, control flows implicitly to the next Block. 
 

### Calling

Quite apart from how we generate instructions and their structure, above class design lets us clearly implement control structures and function calling. But it does not explain how a function call would actually happen, just how the instruction is defined.

To understand function calling, let us look very briefly at how this is done at the cpu level, and what the c machine does. While branching just tranfers control, calling tranfers control with the intent to resume after the call. That means the address where control resumes is needed. Many cpus push the return address onto a system stack, but some, like arm, store it in a designated register. This shows how the return is really a hidden argument to the functon, much in the same way that the object (self) is  ahidden parameter in object oriented languages.

The c machine, imlemented by the compiler, uses registers, the stack or a combination of them, depending on cpu, to pass function arguments. If a function uses local variables, these are usually stored on the stack, and the area designated for this is called the frame. All this moving about of data to and from registers and the stack is what lets the programmer deal in the language concepts, essentially is the implementation of the c machine.

Off course we need to pass objects in the object machine too, and it is quite strightforward to define an array for this. The question is more how to pass the implicit information, since we want to do this too in an object oriented fashion.

#### Message

Because function must often be resolved at runtime, one often speaks of sending messages in object oriented languages. So to make all information passed explicit, in an oo fashion, we define a Message class with the following attributes:

- Name, the name of the function to be called on the object *self*
- *Self* , the object receiving the message
- arguments, an array object containing the arguemnts in order. Arguements are typed as described above.
- return address
- exceptional return address
- Frame object for local and temporary variables


To implement message pasing we then just need to create a Message object and send it. We go into the sending part below, but once it is sent, the return address store, and a method been found, we just need to transfer control.

In analogy to the c example, it is up to the method to create and store the Frame object. So the Frame is not set by the called, even there is a space in the message. The reason for this is so we have the Frame stored in a defined place.

### Sending

Sending a message involves a little more than the calling descibed above. For one thing the Message has to be created. For this the machine keeps a list of unused messages and so the process starts by taking the first free of the list.

When this happens, we are still processing the current message, so we call the next massage NewMessage. NewMessage could be a temporary variable, ie stored inthe frame, but because we need to access it's data we make it a seperate object. Gathering the arguements, the receiver and message name are straigh forward and we can issue the Send instruction which will be covered in more detail below. Sending a message is guaranteed to result in a function call. We guarantee this my calling *message_missing* if no message is found.

We note that we now have exactly four objects whose data the machine accesses. These are

- current message
- self
- frame
- next message

This will become important when defining the instruction set.

### Exceptions

As we have seen, the return address of a regualr return is a hidden argument in the Message. To implement exceptions we need to be able to "return" to a point across several function invocations, but in essence it is the same thing. We store the last exception address in the Message. This may be changed and setting it is equivalent to the language concept catch that many languages have. If it is not set in a Method, the Message creation copies the old address over, and so when an exception is raised, the return is over as many Method invokation as the exception address has not been set.

A side effect of this object oriented design is the eae with which such a raise/catch may be implemented. We do not need to unwind any cryptic stack frames or free any memory or indeed keep book of anything to be free or cleaned up. Messages and Frames are normal objects and when they can not be reached they will be collected. Collection of Messages and Frames makes them available for use for new Method invokations.

