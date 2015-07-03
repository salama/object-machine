## Types and Classes

In our design, objects are made up of Values. We will define Values in a little, but just to clarify,
objects do not contain other objects, only references. In other words true aggregation of objects
is not supported, only association.

Another common feature for all objects is that they have a class, and without using class
inheritance this will default to Object. Unlike other designs, we do not store the class directly
in the object, but use a class called Layout as a mediator. Still, the normal object oriented
assumption is enforced, namely that the class of an object may not be changed during it's life-cycle.
More about the Layout below.

### Values

In the machine design we make the distiction between objects and values explicit.
Values may be easiest to understand in terms of the implementation, they are stored in a machine word.
Conceptually they represent different entities from objects, as

- they can not be changed
- same means identical ie == and === always resolve to the same
- they can not have instance variables

Values have type, and this type is known at run-time. As we mentioned in the first chapter,
early implementations used to tag the machine word with a bit, signaling this type.
This works for the minimal two basic types of integer and reference but has several drawbacks, it:

- is not extensible. It uses a bit to store a possible of two values. One could arguably use more bits, but that would increase below issues, while not resolving all
- makes  data exchange difficult, as external api's may use the full width of the word, not minus one bit. In network traffic and operating system calls this can cause problems
- can never work for floats, as floats use the whole of the machine word in a way that it is not easy to pinch a bit.

All three of those points are unacceptable, and so we choose to encode the value's type in an external fashion.
This is explained in the layout section below. In a current implementation we have 4 bits available
for the value type and thus much room for extension.

To repeat, objects are made up of values. Values are represented by immutable binary patterns that are nevertheless typed. In our implementation value types are stored external to the value, but in a silicon implementation it would be beneficial to store the type with the value.

### Layout and Class

Traditionally objects have a minimum of one association, the class. In some systems the class serves
the double role of specifying what variables/methods an object has and how those are arranged.
We nevertheless want to design an open system, so it should be possible to add instance variables
and methods to objects at run-time. And while it is possible to add this to classes dynamically,
it can easily be somewhat imprecise (having some objects of a class that have a certain variable,
but others not), and easily inefficient (when changing all objects of a class to hold nil values, eg).

Instead of associating the object with it's Class directly, we associate it with it's Layout and
the Layout with the Class it represents. The diagram below illustrates this,
as an object (not class) diagram.

![Object diagram](http://yuml.me/8ea6a0c7)

Each object has a type word, which encodes the types (type1...typeN) of the values the object holds.
Also each object holds an object reference to the layout for this Object. An object *may* hold
any amount of instance variables (logically).
The Layout, being an Object, also has type and layout, but detail is ommitted here for clarity.
The objects Layout holds the names of all the objects instance variables. The Layout also holds a
reference to the class for the Object.

As we want a flexible design, the Class object is mutable at any time. The programmer may add,
or remove variables and methods, even change the inheritance of the class. But it is important
to note that the Layout is **immutable**. This means that when the programmer adds an instance
variable, a **new** Layout is created. This is so that the Layout of other existing objects stays
valid, and off course if or when there are no such other objects the layout will be collected.

### Meta and Eigenclass

To understand the concepts of Meta and Eigenclass it helps to look at what a Class is for.
Instead of defining methods on each object separately, as some languages do, a Class lets
us define methods for a whole group, or class, of objects. The class has a name and it may
derive methods from super-classes, but mainly the class syntax lets us define methods for
a group of objects, not single objects. To implement a machine, it is necessary to define
methods on single objects, and to fit that with the class idea, it is necessary to have
classes that have just one instance (or at least seem to).

The Meta-class was introduced in Smalltalk as the class that is the class of the class of an object.
Instances of Meta-classes were classes and each class had it's own meta-class.
Such is was possible to use the class concept to define methods on singleton objects.
As was realized in ruby, this was unnecessarily complicated as the meta-class is more a concept,
and the only meta-class needed is the Class object.

For our machine we define the Eigenclass of an object to be the object.
Thus any object may act itself in some ways as a class, and the way it does this
is by having instance variables that are Methods. We will define Methods precisely
in the next chapter, but as the name says, they represent callable objects that hold executable code.

When this idea of the Eigenclass is applied to Class instances, we can see that class methods
are in fact methods defined in the class object. Much in the same way as class variables
are instance variables on the class object.

To realize this idea of the Eigenclass we extend the traditional definition of an object.
Instead of just holding instance data and a class reference, an object may have instance variables,
instance methods, and the class reference.
To resolve a method we will start with the objects instance methods, before proceeding to the
class and up the tree.
