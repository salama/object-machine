#### Overview

Soml, like ruby, has open classes. This means that a class can be added to by loading another file
with the same class definition that adds fields or methods. The effect of this is that in designing
the runtime, we can concentrate on a minimal function set.

This means all the functionality the compiler need to get the job done, mostly class and type
structure related functionality with it's support.

### Value and Object

In soml object is not the root of the class hierarchy, but Value is. Integer, Float and Object are
derived from Value. So an integer is *not* an object, but still has a class and methods, just no
instance variables.

### Layout and Class

Each object has a layout that describes the instance variables and types of the object. It also
reference the class of the object. Layout objects are constant, may not be changed over their
lifetime. When a field is added to a class, a new layout is created.

A Class describes a set of objects that respond to the same methods (methods are store in the class).
A Layout describes a set of objects that have the same instance variables.

### Method, Message and Frame

The Method class describes a declared method. It carries a name, argument names and types and
several description of the code. The parsed ast is kept for later inlining, the register model
instruction stream for optimisation and further processing and finally the cpu specific binary
represents the executable code.

When Methods are invoked, A message object (instance of Message class) is populated. Message objects
are created at compile time and form a linked list. The data in the Message holds the receiver,
return addresses, arguments and a frame. Frames are also created at compile time and just reused
at runtime.

### Space and support

The single instance of Space hold a list of all Classes, which in turn hold the methods.
Also the space holds messages will hold memory management objects like pages.

Words represent short immutable text and other word processing (buffers, text) is still tbd.
Lists are number indexed, starting at one, and dictionaries are mappings from words to objects.
